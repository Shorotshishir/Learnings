# Networking In Unity
- Can be called only from the `Main Thread`
- A WebRequest can only begin from `Main Therad`

## Which Means
- `Code Example 1` will show errors, as we are calling `UnityWebRequest` from a Different Thread. 

```cs
// Code Example 1
public class Controller : MonoBehaviour
    {
        // Start runs in Main Thread
        private void Start()
        {
            
            Task.Run(async delegate { await GetNetworkData(); });
        }

        // GetNetworkData runs in another thread
        private async Task GetNetworkData()
        {
            var uri = "google.com";
            var response = UnityWebRequest.Get(uri);
            try
            {
                await response.SendWebRequest();
            }
            catch (Exception e)
            {
                Console.WriteLine(e);
                throw;
            }
        }
    }
```
- `Code Example 2` will show errors, as we are calling `SendWebRequest` from a Different Thread. 

```cs
public class Controller : MonoBehaviour
    {
        // Start runs in Main Thread
        private void Start()
        {
            var uri = "google.com";
            var response = UnityWebRequest.Get(uri);
            UniTask.Run(async delegate { await GetNetworkData(response); });
        }

        // GetNetworkData runs in another thread
        private async UniTask GetNetworkData(UnityWebRequest response)
        {
            try
            {
                await response.SendWebRequest();
            }
            catch (Exception e)
            {
                Console.WriteLine(e);
                throw;
            }
        }
    }
```

## Sending Web Request Asynchronusly 
- Need a third party Library ([UniTask](https://github.com/Cysharp/UniTask))
- Install the package and add namespace to your sourcecode
- Use it like this
```cs
using Cysharp.Threading.Tasks;

public class Controller : MonoBehaviour
{
    // Start runs in Main Thread
    // remember Start is an Event Function, thus 
    // we need to use the event function specific async keywords.
    private void Start()
    {
        var uri = "google.com";
        var response = UnityWebRequest.Get(uri);
        GetNetworkData(uri).Forget(); // triggers async from the main thread without await keyword
    }

    // GetNetworkData runs in another thread
    private async UniTask GetNetworkData(UnityWebRequest response)
    {
        using (var response = UnityWebRequest.Get(uri))
        {
            if (response.isHttpError || response.isNetworkError)
            {
                Debug.Log(response.responseCode);
            }
            else
            {
                await response.SendWebRequest();
                Debug.Log($"Response Code {response.responseCode}");
                var jsonResponse = response.downloadHandler.data;
                var data = PostsList.GetJson(jsonResponse);
                Debug.Log($"data.count {data.count}");
            }
        }
    }
}
```