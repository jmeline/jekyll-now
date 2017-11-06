---
title: Fubutransportion
---

I have been learning about an open source service bus that our company uses called FubuTransportation. It is intented to be a low friction, or an easy to setup and use, service bus.

You can think of a service bus as the Ethernet of SOA (service-oriented architecture). SOA is a software design where services are provided to other components through a communication protocol over a network. A service bus should be loosely coupled software components (a.k.a services) that are expected to be independently deployed, running, heterogeneous, and disparate within a network.

Some of the benefits of using a service bus, over using HTTP services such as REST, include reliable messaging and safe handling of fail conditions (i.e. a bad connection)

Lets get started with our simple fubutransport app. In this simple "Hello World" equivalent of a message bus example, we will create two URI's that will send a ping message and then a pong message will be returned.

First create a console C# project and here are the dependencies you will need:

{% highlight xml linenos %}
  // packages.config
  <?xml version="1.0" encoding="utf-8"?>
  <packages>
    <package id="FubuCore" version="2.0.1.321" targetFramework="net461" />
    <package id="FubuMVC.Core" version="3.1.0" targetFramework="net461" />
    <package id="HtmlTags" version="2.1.0.183" targetFramework="net461" />
    <package id="LightningDB" version="0.9.9" targetFramework="net461" />
    <package id="Newtonsoft.Json" version="9.0.1" targetFramework="net461" />
    <package id="StructureMap" version="4.4.3" targetFramework="net461" />
    <package id="System.Reactive.Core" version="3.1.1" targetFramework="net461" />
    <package id="System.Reactive.Interfaces" version="3.1.1" targetFramework="net461" />
    <package id="System.Reactive.Linq" version="3.1.1" targetFramework="net461" />
    <package id="System.Reactive.PlatformServices" version="3.1.1" targetFramework="net461" />
  </packages>
{% endhighlight %}

### Bootstrapping a fubutransportation app

  A successful application needs to know who to communicate with. These are defined as **Channels**. Another term that will come up is **Transports**, which are the means by which messages are passed to a fubuMVC Node. Currently fubutransportation only supports LightningDB for its transport. This will make more sense after we work through this simple ping pong app that will send a ping message which will then return a pong message. Create a settings class that contains the routes you would like to work with. Let's get started.

{% highlight csharp linenos %}
  // PingPongSettings.cs
  public class PingPongSettings
  {
      public Uri Pinger { get; set; } = "lq.tcp://localhost:2352/pinger".ToUri();
      public Uri Ponger { get; set; } = "lq.tcp://localhost:2353/ponger".ToUri();
  }
{% endhighlight %}

Now lets create the messages that I want to be passing around. In this case, it is a simple class with a Message property of type string.

{% highlight csharp linenos %}
  // PingPongMessage.cs
  public class PingMessage
  {
      public string Message { get; set; }
  }

  public class PongMessage
  {
      public string Message { get; set; }
  }
{% endhighlight %}

Once those messages are constructed, the next step is to create a class that utilizes the PingMessage and PongMessage with the PingSettings class. Earlier, we mentioned the concept of channels. Lets define some channels for our fubutransportation app can use. In the next section of code, we're defining what messages the channel "Ponger" will accept and will explicitly mark Pinger to monitor for incoming messages. From the documentation, "While you can always send messages to any configured channels, you must explicitly mark which channels should be monitored for incoming messages to the current node." We will create the PongApp that will simply listen for messages.

{% highlight csharp linenos %}
  // PingPongApp.cs
  public class PingApp : FubuTransportationRegistry<PingPongSettings>
  {
      // Configuring Ponger to accept messages PingMessage
      Channel(x => x.Ponger)
        .AcceptsMessage<PingMessage>()
        .AcceptsMessage<PongMessage>();
      // Listen for incoming messages from Pinger
      Channel(x => x.Pinger)
        .ReadIncoming();
  }

  public class PongApp : FubuTransportationRegistry<PingPongSettings>
  {
      Channel(x => x.Ponger).ReadIncoming();
  }
{% endhighlight %}

Since being able to pass messages is a nice feature and all, it would be nice to be able to handle and process those messages. In FubuMVC, message handling is done with handler actions. Under the covers, FubuMVC uses [StructureMap 4.0's type scanning support](http://structuremap.github.io/registration/auto-registration-and-conventions/) FubuTransportation will search for naming conventions that contain **Handler** or **Consumer**.

The next stage is to create a handler for our Ping and Pong messages. I will explain the code afterwards.

{% highlight csharp linenos %}
 // PingPongHandlers.cs
 public class PingPongHandler
 {
   public PongMessage Handle(PingMessage ping)
   {
     Console.WriteLine("Recieved ping message: " + ping.Message);
       return ping.Message == "ping"
         ? new PongMessage { Message = "pong" }
         : new PongMessage { Message = string.Empty };
   }
   public PingMessage Handle(PongMessage pong)
   {
     Console.WriteLine("Recieved pong message: " + pong.Message);
     return pong.Message == "pong"
       ? new PingMessage { Message = "ping" }
       : new PingMessage { Message = string.Empty };
   }
 }
{% endhighlight %}

Two handlers are created to accept their respective message types. Each message handler is identically named but its identity is determined by its parameters. If any messages of type **PingMessage** are consumed, this method will be used to accept such messages. I am forcing each message to return a specific response message to simulate a ping-pong interaction. If any of the messages are sent out of the cadence of ping then pong, and then ping again, will returned with an empty message.

Lets see this in action. I am going to simply create a xunit test to demonstrate message passing.

{% highlight csharp linenos %}
  // helloWorld.cs
  public class HelloWorld
  {
     public HelloWorld()
     {
       DeleteDir(Directory.GetCurrentDirectory() + "/fubutransportationqueues.2352");
       DeleteDir(Directory.GetCurrentDirectory() + "/fubutransportationqueues.2353");
     }

     public void DeleteDir(string path)
     {
       if (Directory.Exists(path))
       {
         Directory.Delete(path, true);
       }
     }

     [Fact]
     public async Task request_reply_example()
     {
        var pinger = FubuRuntime.For<PingApp>();
        var ponger = FubuRuntime.For<PongApp>();
        var bus = pinger.Get<IServiceBus>();

        PongMessage pong = await bus.Request<PongMessage>(new PingMessage { Message = "ping" });
        PingMessage ping = await bus.Request<PingMessage>(pong);
        pong.Message.ShouldBe("pong");
        ping.Message.ShouldBe("ping");
        pinger.Dispose();
        ponger.Dispose();
    }
  }
{% endhighlight %}

The **DeleteDir** method simply goes through and removes the existing queues. As the test runs, you will see that the
**guaranteed delivery** works by maintaining a queue in our bin directory. When FubuTransportation starts up, we want to run messages that haven't been consumed yet.

We create a fubuRuntime of our PingApp and our PongApp and from the PingApp, we pull the service bus. Even though the ponger variable isn't explicitly used, it is still needed so that pong messages will be handled. We are emulating the interaction between two systems via the service bus. This service bus pulled from PingApp can accept both ping and pong messages. This setup allows us to freely send both messages around to playtest message passing and recieving.

We create an async test to allow us to slow down and see the messages that are passed and returned from each other. We can see that the bus requests a pong message and accepts a ping message as its parameters. The ping message handler recieves the ping messages and sets the message on the pong message to be "Pong". Pretty neat interaction.

### What happened?

Sometimes messages aren't passed properly and debugging can be a real headache. There are a number of ways to debug transport errors. The first one is to check the diagnostics page. When our app is running, we can head on over to localhost:2353/_fubu (we set this above in the HelloWorldSettings.cs) and take a look at the message handlers. Our app will let us know which handlers are registered. If we checkout the **LightningQueues** section, we can check if there are any errors.

If all looks fine on that end, there could be an issue with fubutransport trying to locate a suitable queue to go into. This will require building fubutransport locally and then debugging through the "FindContinuation" method in the fubutransportation code. Most of the time, this is completely unnecessary but it is interesting to deep dive into the working gears of this service bus.

Make sure to double check that you have the appropriate consumers/handlers setup so that messages can find a home.

This concludes the dive into fubutransportation. You can find the code on my [github](https://github.com/jmeline/FubuMVC/tree/master/src/FubuTransportation)
