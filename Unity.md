Linux execute game
```sh
chmod +x nameOfGame.x86
./nameOfGame.x86 -batchmode
```

Build Exclusion 1 (Code included)
```c#
[Conditional("UNITY_STANDALONE_LINUX"), Conditional("UNITY_EDITOR")]
public void ServerFunction()
{
    print("im server");
}
```

Build Exclusion 2 (Code excluded)
```c#
#if UNITY_STANDALONE_LINUX || UNITY_EDITOR
    Debug.Log("server");
    ServerFunction();
#endif

#if UNITY_STANDALONE_LINUX || UNITY_EDITOR
    public void ServerFunction()
    {
        print("im server");
    }
#endif
```

Server check if
```c#
if (SystemInfo.deviceName == "domain.server.com")
{
    AutoClickServer(); // 1 host
}
```
