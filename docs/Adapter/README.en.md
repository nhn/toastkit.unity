# Adapter

🌏 [한국어](README.md)

## Overview

IdPs, such as Facebook or Google, provide Unity SDK to apply IdP functions faster and more easily in applications under development for Unity. However, as each IdP has different API, sufficient learning and time is required to implement each and every IdP function. 
TOAST Kit Adapter provides a common interface so as to easily apply different IdP fuctions.

## Specification

### Unity support version

* 5.6.6 or higher

### Support IdP

Each IdP SDK must be downloaded and installed manually.

* Google Play Games plugin for Unity
    * Tested version
        * 0.9.56
        * 0.9.57
        * 0.9.63
    * [Download](https://github.com/playgameservices/play-games-plugin-for-unity)
* Facebook SDK for Unity 
    * Tested version
        * 7.15.0
        * 7.15.1
    * [Download](https://developers.facebook.com/docs/unity/downloads)



## 🔨 API

### IsSuccess

Use AdapterError object to see if it is successful. 
For more information regarding AdapterErrorCode, see the ErrorCode itmes as below.

#### API

```cs
static bool IsSuccess(AdapterError error)
```

#### Example

```cs
private void SampleIsSucces(AdapterError error)
{
    if (ToastKitAdapter.IsSuccess(error) == true)
    {
        Debug.Log("success");
    }
    else
    {
        Debug.Log(string.Format("failure. error:{0}", error));
    }
}
```

### Login

Try login to IdP, by using IdP name and additional information.
The IdP names supported by TOAST Kit Adapter are provided via the ToastKitAdapterType.Idp class. 

> [Note]
> To login to Facebook SDK, Facebook authority information is required, which can be configured at additional information by using the `facebook_permissions` key. If Facebook authority information is not configured, the default value `[public_profile, email]` is configured. For more details, see Example as below. 

#### API

```cs
static void Login(string idpName, Dictionary<string, object> additionalInfo, Action<AdapterError> callback)
```

#### Example

```cs
private void SampleLogin(string idpName)
{
    Dictionary<string, object> additionalInfo;
    
    switch (idpName)
    {
        case ToastKitAdapterType.Idp.Facebook:
        {
            var facebookPermissionList = new List<string> { "public_profile", "email", "user_friends" };
            additionalInfo = new Dictionary<string, object>();
            additionalInfo.Add("facebook_permissions", facebookPermissionList);
            break;
        }
        case ToastKitAdapterType.Idp.Gpgs:
        default:
        {
            additionalInfo = null;
            break;
        }
    }
    
    ToastKitAdapter.Idp.Login(ToastKitAdapterType.Idp.Facebook, additionalInfo, (error) => 
    {
        if (ToastKitAdapter.IsSuccess(error) == true)
        {
            Debug.Log("success");
        }
        else
        {
            Debug.Log(string.Format("failure. error:{0}", error));
        }
    });
}
```

### Logout

Try logout of IdP.

#### API

```cs
static void Logout(string idpName, Action<AdapterError> callback)
```

#### Example

```cs
private void SampleLogout()
{
    ToastKitAdapter.Idp.Logout(ToastKitAdapterType.Idp.Facebook, (error) => 
    {
        if (ToastKitAdapter.IsSuccess(error) == true)
        {
            Debug.Log("success");
        }
        else
        {
            Debug.Log(string.Format("failure. error:{0}", error));
        }
    });
}
```

### LogoutAll

Try logout of all IdPs.
If an error occurs before all IdPs are logged-out, stop processing logouts and return the error.

#### API

```cs
static void LogoutAll(Action<AdapterError> callback)
```

#### Example

```cs
private void SampleLogoutAll()
{
    ToastKitAdapter.Idp.LogoutAll((error) =>
    {
        if (ToastKitAdapter.IsSuccess(error) == true)
        {
            Debug.Log("success");
        }
        else
        {
            Debug.Log(string.Format("failure. error:{0}", error));
        }
    });
}
```

### GetAuthInfo

Query authentication information of IdP. 
Return AccessToken for Facebook, and return ServerAuthCode for Google. 
When authentication information is empty, enable DebugLogEnabled and check logs.

#### API

```cs
static void GetAuthInfo(string idpName, Action<string> callback)
```

#### Example

```cs
private void SampleGetAuthInfo()
{
    ToastKitAdapter.Idp.GetAuthInfo(ToastKitAdapterType.Idp.Facebook, (facebookAuthInfo) => 
    {
        Debug.Log(string.Format("authInfo:{0}", facebookAuthInfo));
    });
}
```

### GetProfile

Query profile information of IdP.
Basic profile information is as follows.

* id
* name
* email


#### API

```cs
static void GetProfile(string idpName, Action<Dictionary<string, object>> callback)
```

#### Example

```cs
private void SampleGetProfile()
{
    ToastKitAdapter.Idp.GetProfile(ToastKitAdapterType.Idp.Facebook, (facebookProfile) =>
    {
        if (facebookProfile == null)
        {
            Debug.Log("Facebook profile is null.");
        }
        else
        {
            foreach (KeyValuePair<string, object>kvp in facebookProfile)
            {
                Debug.Log(string.Format("{0}:{1}\n", kvp.Key, kvp.Value));
            }
        }
    });
}
```

### GetLoggedInIdpList

Query all logged-in IdP names.

#### API

```cs
static List<string> GetLoggedInIdpList()
```

#### Example

```cs
private void SampleGetLoggedInIdpList()
{
    var loggedInIdpList = ToastKitAdapter.Idp.GetLoggedInIdpList();
    foreach (var loggedInIdp in loggedInIdpList)
    {
        Debug.Log(string.Format("loggedInIdp:{0}", loggedInIdp));
    }
}
```

### GetUserId

Query UserId information of IdP.

#### API

```cs
static string GetUserId(string idpName)
```

#### Example

```cs
private void SampleGetUserId()
{
    var facebookUserId = ToastKitAdapter.Idp.GetUserId(ToastKitAdapterType.Idp.Facebook);
    Debug.Log(string.Format("facebookUserId:{0}", facebookUserId));
}
```

### ErrorCode

| Error | Error Code | Description |
| --- | --- | --- |
| SUCCESS | 0 | Success. |
| ADAPTER_NOT_FOUND | 1 | Cannot find adapter. Configure an adapter. |
| NOT_LOGGED_IN | 2 | Not logged-in. Log in first to call API. |
| USER_CANCELED | 3 | Cancelled by user. |
| EXTERNAL_LIBRARY_ERROR | 4 | Error in external library. |