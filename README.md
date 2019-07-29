# Create-Azure-Function-to-Call-WebAPI
Call a webAPI which is hosted in Azure platform from Azure Function in a scheduled time. 

#r "Newtonsoft.Json"
using System;
using Newtonsoft.Json;
using System.Text;

public static async Task Run(TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    var res=await CallHttpClient(log);
    
}

public static async Task<string> CallHttpClient(TraceWriter log)
{
    using (var httpClient = new HttpClient())
    {
       
Dictionary<string, string> dictionary = new Dictionary<string, string>();

dictionary.Add("SiteUrl", "https://SiteCollection/sites/SiteCollectionName/Subsites");       
dictionary.Add("AlertTitle", "Daily Report");
dictionary.Add("ListName", "Upstream Operations Daily Report");


string json = JsonConvert.SerializeObject(dictionary);
var requestData = new StringContent(json, Encoding.UTF8, "application/json");

var response = await httpClient.PostAsync(String.Format("http://XXXXwebapi.azurewebsites.net/api/Email"), requestData);
var result = await response.Content.ReadAsStringAsync();

log.Info($"C# Timer trigger function executed at: {result}");
var resultData="OK";
return resultData;

    }
}










