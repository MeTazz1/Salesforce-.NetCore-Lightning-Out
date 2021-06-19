# Salesforce-.NetCore-Lightning-Out
A clean &amp; easy to understand .NetCore project implementing Salesforce Lightning Out. It is time to get your Salesforce experience outside Salesforce, my friends! 

Lightning out uses a JS file that is hosted in your org (nothing to add, it's already there!). You would typically find that file here: 
https://your-org.my.salesforce.com/lightning/lightning.out.js

In the .NetCore solution (or any stack you are using), you will need to import that file in your code in order for Lightning Out to work. 
Lightning out is basically a bridge that allows you to bring Salesforce stuff outside of Salesforce. They will do a better job at explaining it here: 
https://developer.salesforce.com/docs/component-library/documentation/en/lwc/lightning_out
Please note that it is still in Beta (as of June 2021). 

Once this bridge is created using a Connected App, you'll need to authenticate to that connected app in your external service using OAuth (code is included, do not worry). 
After authentication, you'll be able to initiate your Lightning Out components and view/use outside of Salesforce. Steps are described below. 

# Implementation
This is a 2 step process: Salesforce, and .NetCore

## Salesforce side: 
- First, you need to create a Connected App in your Salesforce Org. Nothing fancy, just a connected app allowing OAuth authentication, with the "Access and Manage your data (API)" OAuth scope.
- (Optional) Based on how your org is configured, you might need to add a Remote Site Setting and a CORS Setting, allowing Salesforce to emit/receive content from the given URL. 
- For a smoother process, use "Relax IP Address". Once the build is approved, do not forgot to restrict the IP Addresses to your known hosts. 
- Create a new Aura App named "lightningout" with the following content: 
```
<aura:application access="GLOBAL" extends="ltng:outApp" implements="ltng:allowGuestAccess" >
    <aura:dependency resource="c:portalClarificationCmp"/> 
</aura:application>
```
- Create a new Aura Component named "lightningoutCmp" with the following content: 
```
<aura:component controller="lightningOutController" implements="force:appHostable,flexipage:availableForAllPageTypes" access="global" >
    <aura:attribute name="requestObj" type="Map" />
    <aura:handler name="init" value="{!this}" action="{!c.doInit}"/>

    <h1>Welcome to your Aura component using Lightning Out</h1>
    <!-- You can add any other Aura components or LWCs here, once you Aura component is initialized using the doInit function.  -->
</aura:component>
```
In the example above, "requestObj" is a map that contains all the parameters being sent from the .NetCore solution. For example, it could be the .NetCore userId/sessionId and the host URL, if you need to have some type of logic around the connected User. 

## .Net side: 
- Clone this repo and open the Visual Studio Solution. 
- Everything is pre-configured to use the Aura App and Aura Component you have created above. You will only need to update the code with your connected app Client Id, Secret, and your salesforce Username and Password. 


