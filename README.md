# Azure SDK for .NET: how to create a ResourceGroup (Application Registration)

## 1. Register the application in Microsoft Entra ID

We create the Service principal with the following command:

```
az ad sp create-for-rbac --name "myserviceprincipal" ^
--role contributor ^
--scopes /subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXX ^
--sdk-auth
```

We navigate to Azure **Microsoft Entra ID**

![image](https://github.com/luiscoco/Azure_SDK_Create_ResourceGroup_Legacy_Authorization/assets/32194879/024bd812-9804-42d7-996d-50647bdbc204)

First we get the **Tenant ID**

![image](https://github.com/luiscoco/Azure_SDK_Create_ResourceGroup_Legacy_Authorization/assets/32194879/e5784699-9270-4021-a461-60fc283e7324)

We enter **Microsoft Entra ID** and we click on **App registrations**

![image](https://github.com/luiscoco/Azure_SDK_Create_ResourceGroup_Legacy_Authorization/assets/32194879/d00d9bcc-1755-4aa3-9905-da41a97ffa11)

We click on the "myserviceprincipal", the service principal we created at the begining of this section 2

We copy the **clientId** and (optionally) you can also get from here the **tenantId**, we paste both values in the application variables **clientId** and **tenantId**

![image](https://github.com/luiscoco/Azure_SDK_Create_ResourceGroup_Legacy_Authorization/assets/32194879/d513cb7c-14f7-4c26-8df1-595c3dd41963)

We create a new secret clicking on Client credentials link

![image](https://github.com/luiscoco/Azure_SDK_Create_ResourceGroup_Legacy_Authorization/assets/32194879/01c48b28-ec72-4200-80dd-55d1e3e56084)

We press in the **New client secret** button

![image](https://github.com/luiscoco/Azure_SDK_Create_ResourceGroup_Legacy_Authorization/assets/32194879/83c1c8e8-cf0c-491d-9116-cd6933eff4a3)

We copy the **Secret** value to paste in the application **clientSecret** variable 

We navigate in Azure Portal to the **Subscriptions** service and we copy the subscription  subscriptionId 

![image](https://github.com/luiscoco/Azure_SDK_Create_ResourceGroup_Legacy_Authorization/assets/32194879/4cd4bad9-fb70-4e33-9a53-857146eddd5b)

![image](https://github.com/luiscoco/Azure_SDK_Create_ResourceGroup_Legacy_Authorization/assets/32194879/e1384059-1c7a-4930-856a-5f2466cf75bd)

## 2. We assign the Contributor role to the Application

<img width="1919" height="878" alt="image" src="https://github.com/user-attachments/assets/06b2ba51-d0dd-4a7c-942c-eccd04d8fa24" />

## 3. We assign the Contributor role to the Subscription

<img width="1916" height="905" alt="image" src="https://github.com/user-attachments/assets/802f4ea8-281c-4696-98b6-03cd23c7d27c" />

<img width="1919" height="902" alt="image" src="https://github.com/user-attachments/assets/d795bd8a-e621-4af1-93d2-555317e07076" />

<img width="1919" height="913" alt="image" src="https://github.com/user-attachments/assets/2a5729f1-868e-4ca4-b8fb-0945242ad86b" />

<img width="1919" height="908" alt="image" src="https://github.com/user-attachments/assets/d754689e-700a-4553-8114-ffb371ced3eb" />

## 4. Input the application source code

Open VSCode and create a new C# application with this command:

```
dotnet new console --framework net10.0
```

Add the dependencies with the commands:

```
dotnet add package Azure.Identity
```

```
dotnet add package Azure.ResourceManager
```

```
dotnet add package Azure.ResourceManager.Storage
```

We review the csproj file to see the added libraries

```csproj
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net10.0</TargetFramework>
    <ImplicitUsings>enable</ImplicitUsings>
    <Nullable>enable</Nullable>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Azure.Identity" Version="1.14.2" />
    <PackageReference Include="Azure.ResourceManager" Version="1.13.2" />
    <PackageReference Include="Azure.ResourceManager.Storage" Version="1.4.4" />
  </ItemGroup>

</Project>
```

Input the C# source code for creating a new Azure ResourceGroup with Azure SDK for .NET

```csharp
namespace AzureResourceGroupCreation
{
    class Program
    {
        static async Task Main(string[] args)
        {
            // Replace with your Azure AD application details
            string clientId = "dda9be61-XXXXXXXX";
            string clientSecret = "2FR8Q~XXXXXXXXXXXX";
            string tenantId = "e099cebd-XXXXXXXXXXXXXXXXXX";
            string subscriptionId = "e5bd93f3-XXXXXXXXXXXXXXXXXXX";

            string resourceGroupName = "myNewRGLuis";
            string location = "westeurope";

            try
            {
                // Authenticate using ClientSecretCredential
                var credential = new ClientSecretCredential(tenantId, clientId, clientSecret);

                // Initialize the ARM client
                ArmClient armClient = new ArmClient(credential, subscriptionId);

                // Get the subscription
                SubscriptionResource subscription = await armClient.GetDefaultSubscriptionAsync();

                // Define the resource group parameters
                ResourceGroupData resourceGroupData = new ResourceGroupData(location);

                // Create the resource group
                ResourceGroupCollection resourceGroups = subscription.GetResourceGroups();
                ArmOperation<ResourceGroupResource> operation = await resourceGroups.CreateOrUpdateAsync(
                    WaitUntil.Completed,
                    resourceGroupName,
                    resourceGroupData
                );

                ResourceGroupResource result = operation.Value;
                Console.WriteLine($"✅ Resource group created: {result.Data.Name} in {result.Data.Location}");
            }
            catch (Exception ex)
            {
                Console.WriteLine($"❌ Error: {ex.Message}");
            }
        }
    }
}
```

## 5. We run the application in VSCode

<img width="1919" height="641" alt="image" src="https://github.com/user-attachments/assets/cffd232f-e90e-4a18-9c56-bee99c852ff6" />



