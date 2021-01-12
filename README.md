# spfx-teams-meetings

## Summary

Solution to repro the issue with using SharePoint Framework solutions as Teams meetings apps.

## Prerequisites

- [SharePoint app catalog](https://docs.microsoft.com/sharepoint/dev/spfx/set-up-your-developer-tenant)
- [Sideloading apps in Teams enabled](https://docs.microsoft.com/sharepoint/dev/spfx/integrate-with-teams-introduction#turn-on-side-loading-of-external-apps-in-teams)

## Repro steps

1. Deploy .sppkg
    1. Go to SharePoint app catalog
    1. From the `sharepoint/solution` folder, upload the `spfx-teams-meetings.sppkg` file to SharePoint app catalog and deploy the package globally
1. Deploy the Teams app
    1. Go to Teams @ [https://teams.microsoft.com](https://teams.microsoft.com)
    1. From the left rail, choose **Apps**
    1. From the menu, choose **Upload a customized app** and then **Upload for Contoso**
    1. In the file dialog, from the `teams` folder, select the `spfx-teams-meetings.zip` file
    1. From the list of apps, open the **SPFx Meetings** app and choose **Add to a team**
    1. Select a team and choose **Set up a tab**
    1. When then **SPFx Meetings** dialog opens with a Teams logo, choose the **Save** button
1. Add the app to a meeting
    1. In Teams, switch to Calendar and schedule a meeting with at least one participant
    1. After creating the meeting, select it in the calendar view and choose **Chat with participants**
    1. In the meeting chat, on the top bar, choose **+** and from the list of available apps select **SPFx Meetings**
    1. In the **SPFx Meetings** app info dialog, choose **Add**
    1. When the **SPFx Meetings** app dialog with Teams logo shows, choose **Save**. You'll see error similar to the following:
        > Error: [HTTP]:500 - Internal Server Error [CorrelationId]:428ea09f-90f9-2000-6d2d-33985065ad25 [Version]:16.0.0.20809

        The error is caused by the following request:

        ```text
        POST https://contoso.sharepoint.com/_api/web/hostedapps/add

        {"hostType":"Teams","webPartDataAsJson":"{\"dataVersion\":\"1.0\",\"description\":\"HelloWorld description\",\"id\":\"111951e7-ceb0-4c7b-8f9a-3c4f8991715e\",\"properties\":{\"description\":\"HelloWorld\"},\"instanceId\":\"b1db25f6-6181-4e51-8347-205d9cef6159\",\"title\":\"HelloWorld\"}"}
        ```
