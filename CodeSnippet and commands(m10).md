//Create a new .Net Core console app
dotnet new console --name MessageProcessor --output . 

//Add Nuget packages for queues on Azure Storage
dotnet add package Azure.Storage.Queues --version 12.0.0 

//Build the solution 
dotnet build 

//Replace the code on program.cs with the below given snippets, add your storageConnectionString as well:
using Azure;
using Azure.Storage.Queues;
using Azure.Storage.Queues.Models;
using System;
using System.Threading.Tasks;
public class Program
{
    private const string storageConnectionString = "<storage-connection-string>";
    private const string queueName = "messagequeue";

    public static async Task Main(string[] args)
    {
    }
}

//CodeSnippet to add queue client
QueueClient client = new QueueClient(storageConnectionString, queueName);        
await client.CreateAsync();
Console.WriteLine($"---Account Metadata---");
Console.WriteLine($"Account Uri:\t{client.Uri}");

//Run the solution using the below command
dotnet run 

//Remove the queue client for console WriteLine and add the below CodeSnippet
Console.WriteLine($"---Existing Messages---");
int batchSize = 10;
TimeSpan visibilityTimeout = TimeSpan.FromSeconds(2.5d);
Response<QueueMessage[]> messages = await client.ReceiveMessagesAsync(batchSize, visibilityTimeout);
foreach(QueueMessage message in messages?.Value)
{
Console.WriteLine($"[{message.MessageId}]\t{message.MessageText}"); 
}

//Build the solution
dotnet build 

//Run the solution
dotnet build 

//invoke the DeleteMessageAsync method of the QueueMessage class using the below CodeSnippet
await client.DeleteMessageAsync(message.MessageId, message.PopReceipt);

//add the below code on the main methodConsole.WriteLine($"---New Messages---"); 
string greeting = "Hi, Developer!";
await client.SendMessageAsync(greeting);
await client.SendMessageAsync(greeting);



