```cs
public static async Task<int> Test1(string url)
{
    string json;

    using (FakeHttpClient httpClient = new FakeHttpClient())
        json = await httpClient.GetStringAsync(url);

    return new JsonFeatureCollection(json);
}
public static Task<int> Test2(string url)
{
    using (FakeHttpClient httpClient = new FakeHttpClient())
        return httpClient.GetStringAsync(url).ContinueWith(t => new JsonFeatureCollection(t.Result));
}

public static void Main()
{
    {
        Stopwatch timer = new Stopwatch();
        timer.Start();

        for (int i = 0; i < 100000; i++)
            Test1(sourceUrl).Wait();

        timer.Stop();
        Console.WriteLine(timer.Elapsed); // 00:00:00.0289885
    }

    {
        Stopwatch timer = new Stopwatch();
        timer.Restart();

        for (int i = 0; i < 100000; i++)
            Test2(sourceUrl).Wait();

        timer.Stop();
        Console.WriteLine(timer.Elapsed); // 00:00:04.4958830
    }
}
```

