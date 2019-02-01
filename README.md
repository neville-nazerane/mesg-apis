# **mesg apis for C#** 

## **Installation**

Adding core library (meant for applications):

    dotnet add package mesg.core

Adding service library (meant for service)

    dotnet add package mesg.service
    
## **Usage sample**

    var channel = new Channel("localhost:50052", ChannelCredentials.Insecure);
    var client = new Core.CoreClient(channel);

    var response = client.ListenEvent(new ListenEventRequest{ ServiceID = "webhook", EventFilter = "request" }).ResponseStream;

    Console.WriteLine("Started...");
    while (await response.MoveNext(CancellationToken.None)) {
        client.ExecuteTask(new ExecuteTaskRequest{
            ServiceID = "discord-invitation",
            TaskKey = "send",
            InputData = JsonConvert.SerializeObject(new {email, sendgridAPIKey})
        });
        Console.WriteLine("Waiting for next...");
    }
