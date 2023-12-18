# Azure SDK Create ResourceGroup Legacy Authorization

## 1. Register the application in Microsoft Entra ID

We create the Service principal with the following command:

```
az ad sp create-for-rbac --name "myserviceprincipal" ^
--role contributor ^
--scopes /subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXX ^
--sdk-auth
```

We navigate to Azure **Microsoft Entra ID**

First we get the **Tenant ID**

![image](https://github.com/luiscoco/Azure_SDK_Create_ResourceGroup_Legacy_Authorization/assets/32194879/e5784699-9270-4021-a461-60fc283e7324)

We enter **Microsoft Entra ID** and we click on **App registrations**

![image](https://github.com/luiscoco/Azure_SDK_Create_ResourceGroup_Legacy_Authorization/assets/32194879/d00d9bcc-1755-4aa3-9905-da41a97ffa11)

We click on the "myserviceprincipal", the service principal we created at the begining of this section 2

We copy the **clientId** and (optionally) you can also get from here the **tenantId**

![image](https://github.com/luiscoco/Azure_SDK_Create_ResourceGroup_Legacy_Authorization/assets/32194879/d513cb7c-14f7-4c26-8df1-595c3dd41963)



## 2. Input the application source code

```csharp

```


