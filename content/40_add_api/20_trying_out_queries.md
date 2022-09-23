---
title : "Trying out some queries"
chapter : false
weight : 20
---


**Open the AWS Console** and **click into the amplifyiosworkshop-dev API**. Now we can start poking around with the API.

::::tabs
:::tab{id="us-west-2" label="North America"}
Link to [AWS AppSync web console in Oregon](https://console.aws.amazon.com/appsync/home?region=us-west-2#/apis)
:::
:::tab{id="eu-central-1" label="Europe"}
Link to [AWS AppSync web console in Frankfurt](https://console.aws.amazon.com/appsync/home?region=eu-central-1#/apis)
:::
::::

**Click Queries** in the sidebar on the left.

![appsync queries](/static/images/40-20-appsync-1.png)

This area is AWS AppSync's interactive query explorer. We can write queries and mutations here, execute them, and see the results. It's a great way to test things out to make sure our resolvers are working the way we expect.

### Authenticating to AppSync


::alert[Before we can issue queries, we'll need to authenticate (because our AppSync API is configured to authenticate users via the Amazon Cognito User Pool we set up when we configured the authentication for our app.]{header="Warning" type="warning"}


1. **Click the Login with User Pools button** at the top of the query editor.

2. Look up the value for the **ClientId** field.  You can either use the script below or get the client ID in the console. To use the script, in your terminal, type:

:::code{language=bash}
cd $PROJECT_DIRECTORY
../../scripts/get_app_client.sh
:::
Copy the **Client ID** value returned, for example:
![Client ID](/static/images/40-20-client-id.png)

Alternatively, you can retrieve the ClientID in the Amazon Cognito console:

1.  Click on the link to open the Cognito console in your Region: [Northern America](https://eu-west-1.console.aws.amazon.com/cognito/users/?region=us-east-1#/) or [Europe](https://eu-west-1.console.aws.amazon.com/cognito/users/?region=eu-central-1#/)
2.  Select the User Pool named **amplifyiosworkshopxxxxx_userpool_xxxx-dev** (the xxx is generated randomly and will vary)
3. Click **App Clients** on the left menu, select the client named **amplifyxxxx_app_clientWeb** and click **Show Details** to reveal both the App client id and the App client secret.  Copy the **App client id**.  The correct app client has no secret key as "App client secret".  
![app client id](/static/images/40-20-appsync-2.png)

1. Go back to the **AppSync console**, in the **Query** section Paste the value into the **ClientId** field

1. **Enter your credentials** for the user you created when we added authentication

1. **Click Login**

![appsync authentication](/static/images/40-20-appsync-3.png)

### Trying out some queries

You should now be able to try out the following mutations and queries. Press the orange 'play' button to execute queries and mutations.

**Add a new Landmark** by copy/pasting the following and running the query:

:::code{language=graphql}
mutation CreateLandmark($input: CreateLandmarkDataInput!) {
    createLandmarkData(input: $input) {
        id
    }
}
:::

Open the **Query Variables** section on the bottom and copy / paste the following data:

:::code{language=json}
{ "input" :
    {
        "name": "Lake Umbagog",
        "category": "Lakes",
        "city": "Errol",
        "state": "New Hampshire",
        "id": 9999,
        "isFeatured": true,
        "isFavorite": false,
        "park": "Umbagog National Wildlife Refuge",
        "coordinates": {
            "longitude": -71.056816,
            "latitude": 44.747408
        },
        "imageName": "umbagog"
    }  
}
:::

Click the orange Play button (<i class="far fa-caret-square-right" aria-hidden="true" style="background: orange; color: white;"></i>) to execute the query.

**Get the Landmark**  we just created by running this query:

:::code{language=graphql}
query GetLandmark {
    getLandmarkData(id: 9999) {
        id
        name
        category
        city
        state
        isFeatured
        isFavorite
        park
        coordinates {
            longitude
            latitude
        }
        imageName
    }
}
:::

Click the orange Play button (<i class="far fa-caret-square-right" aria-hidden="true" style="background: orange; color: white;"></i>) to execute the query.

**List all the landmarks** with this query :

:::code{language=graphql}
query ListLandmarks {
    listLandmarkData {
        items {
            id
            name
            category
            city
            state
            isFeatured
            isFavorite
            park
            coordinates {
                longitude
                latitude
            }
            imageName
        }
        nextToken
    }
}
:::

Click the orange Play button (<i class="far fa-caret-square-right" aria-hidden="true" style="background: orange; color: white;"></i>) to execute the query.

Finally, **delete the landmark** we created with this query:

:::code{language=graphql}
mutation DeleteLandmark {
  deleteLandmarkData(input: { id: 9999 }) {
    id
    name
    category
    city
    state
    isFeatured
    isFavorite
    park
    coordinates {
      longitude
      latitude
    }
    imageName
  }
}
:::

Click the orange Play button (<i class="far fa-caret-square-right" aria-hidden="true" style="background: orange; color: white;"></i>) to execute the query.

As you can see, we're able to read and write data through GraphQL queries and mutations and AppSync takes care of reading and persisting data (in this case, to DynamoDB).
