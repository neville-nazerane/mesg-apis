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

    System.Console.WriteLine("Starting...");
    while (await response.MoveNext(CancellationToken.None)) {
        Console.WriteLine(response.Current);
        client.ExecuteTask(new ExecuteTaskRequest{
            ServiceID = "discord-invitation",
            TaskKey = "send",
            InputData = JsonConvert.SerializeObject(new {email, sendgridAPIKey})
        });
        System.Console.WriteLine("Next...");
    }
