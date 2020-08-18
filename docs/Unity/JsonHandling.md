# How to handle JSON data
- Unity has its own Json Utility : [JsonUtility](https://docs.unity3d.com/Manual/JSONSerialization.html)
- This has 3 public functions
    - `ToJson()`[ref](https://docs.unity3d.com/ScriptReference/JsonUtility.ToJson.html)
    - `FromJson()`[ref](https://docs.unity3d.com/ScriptReference/JsonUtility.FromJson.html)
    - `FromJsonOverwrite()`[ref](https://docs.unity3d.com/ScriptReference/JsonUtility.FromJsonOverwrite.html)

- If receiving data from an API as Json, store it in model class
- Use [Json2Csharp](https://json2csharp.com/) converter to Generate a `C#` model class.
- Make sure to click on settings and tick mark on the `Use Fields`
- Copy the model class in your project, create a new `class` file and paste the model class.
- Change the `Root` class name to match the name of the file
- Put `[Serializable]` tag on top of all the classes inside the file. or else unity will not be able to serialize the data received.
- **If forgot to check the `Use Field`, and all variable initializatino looks like properies, remove all the `{get; set;}`**
- **Make them `UnInitialized Fields`** or else your data class will always send blank data.
- To keep things organized, Create a `public` `static` function that will receive the response from an API call and return the model file as data object.
- It looks Something Like this.
```cs
// this is the class that requests data from the API
public class Controller : MonoBehaviour
{
    // Start runs in Main Thread
    private void Start()
    {
        const string uri = "https://tanzims-django-blog.herokuapp.com/api/post/";
        Debug.Log("Start " + GetThreadId());
        GetNetworkData(uri).Forget();
    }
    
    private static async UniTaskVoid GetNetworkData(string uri)
    {
        Debug.Log("GetNetworkData" + GetThreadId());
        using (var response = UnityWebRequest.Get(uri)) 
        {
            await response.SendWebRequest(); // request sent
            Debug.Log($"ResponseCode {response.responseCode}");
             // storing data as byte array,  `response.downloadHandler.text` will save as text directly
            var jsonResponse = response.downloadHandler.data;
            var data = PostsList.GetJson(jsonResponse); // accessing the static method to Parse JsonData
            Debug.Log($"data.count {data.count}"); 
        }
    }
}
```

```cs
// this is the datamodel class, it stores the json data , parse it and returns a DataObject
// In our case, PostsList
[Serializable]
public class Result
{
    public string url;
    public int id;
    public string title;
    public int author;
}

[Serializable]
public class PostsList
{
    public int count;
    public string next;
    public string previous;
    public List<Result> results;
    
    // parser Function, for receiving byte array directly
    public static PostsList DataFromJson (byte[] jData){
        return JsonUtility.FromJson<PostsList>(Encoding.UTF8.GetString(jData));
    }
    // parser function, for receiving text data
    public static PostsList DataFromJson (string jData){
        return JsonUtility.FromJson<PostsList>(jData);
    }
}
```
- `DataFromJson()` is a static function, it uses `JsonUtility` module of Unity, `FromJson()` takes string as parameter and formats and stores it based on the `PostList` class.
- If not sure wheather your data class is storing data as is, you can use a test function like this, this function will take the parsed data and make it a json data again, if the API data and this functions Json Data matches your model is storing data properly.

```cs

public class Controller : MonoBehaviour
{
    // Start runs in Main Thread
    private void Start()
    {
        const string uri = "https://tanzims-django-blog.herokuapp.com/api/post/";
        Debug.Log("Start " + GetThreadId());
        GetNetworkData(uri).Forget();
    }
    
    private static async UniTaskVoid GetNetworkData(string uri)
    {
        Debug.Log("GetNetworkData" + GetThreadId());
        using (var response = UnityWebRequest.Get(uri)) 
        {
            await response.SendWebRequest(); // request sent
            Debug.Log($"ResponseCode {response.responseCode}");
             // storing data as byte array,  `response.downloadHandler.text` will save as text directly
            var jsonResponse = response.downloadHandler.data;
            var data = PostsList.GetJson(jsonResponse); // accessing the static method to Parse JsonData
            Debug.Log($"data.count {data.count}"); 
            TestJson(data);
        }
    }

    // Tester Class
    private static void TestJson(PostsList data)
    {
        var data2Json = JsonUtility.ToJson(data);
        Debug.Log(data2Json);
    }
}
```