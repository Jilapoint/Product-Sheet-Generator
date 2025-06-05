# Automated Product Sheet Generator

A Power Platform **Solution** (“Product Sheet Generator”) combining Power Apps, Power Automate, Azure OpenAI, and SharePoint to automatically generate product sheets (long description, short description, SEO keywords) from a product name, a few technical features, and an optional target audience.

---

## Table of Contents

1. [Project Overview](#project-overview)  
2. [Key Features](#key-features)  
3. [Prerequisites](#prerequisites)  
4. [Installation & Deployment](#installation--deployment)  
   - [1. Configure Azure OpenAI](#1-configure-azure-openai)  
   - [2. Prepare SharePoint](#2-prepare-sharepoint)  
   - [3. Import the Solution into Power Platform](#3-import-the-solution-into-power-platform)  
   - [4. Configure Environment Variables](#4-configure-environment-variables)  
   - [5.The Power Apps Canvas App](#5-deploy-the-power-apps-canvas-app)  
---

## Project Overview

The **Automated Product Sheet Generator** solution allows marketing, sales, or e-commerce teams to quickly produce product sheet content (title, long description, short description, SEO keywords) with a low-code interface. Users input:

- **Product Name**  
- **Specifications** (3–4 bullet points)  
 

Clicking **“Generate Sheet”** in Power Apps triggers a Power Automate flow that sends these inputs to **Azure OpenAI (GPT-4)**. The flow returns:

1. A **long marketing description** (web-style copy)  
2. A **short product description** (≤160 characters)  
3. A set of **SEO keywords**  

Generated sheets are archived in a SharePoint list or library, with metadata stored for auditing and retrieval.

All components—Power Apps, the flow, environment variables—are bundled into a single Power Platform **Solution** named **Product Sheet Generator**.
![image](https://github.com/user-attachments/assets/3f44f6d6-573e-464f-986d-8ff964b6c183)


---

## Key Features

- **Solution-based deployment**: all resources (App, Flow, Environment Variables) live in a single Solution called **Product Sheet Generator**.  
- **Power Apps low-code UI**: guided input for product name, features, and target audience.  
- **Azure OpenAI integration** via a Power Automate connector—no custom backend code required.  
- **JSON-strict prompt**: enforces a fixed output schema and character limits (long ≤570 chars; short ≤160 chars).  
- **SharePoint archiving**: saves generated sheets into a configurable SharePoint list/library using Environment Variables.  
- **Environment Variables**: store the **SharePoint Site URL** and **List/Library Name** rather than hard-coding them in the flow.  
- **Extensible design**: supports adding human approval flows (Power Automate Approvals), multiple languages, or future chatbot integration.  
- **Error handling**: catches invalid JSON or API failures and returns clear error info to Power Apps.

---

## Prerequisites

Before deploying, ensure you have:

1. **Azure Subscription & Azure OpenAI**  
   - A deployed **Azure OpenAI** resource (e.g., `gpt4`).  
   - Note the exact **Deployment ID** (case-sensitive) and **Endpoint** URL (`https://<your-resource>.openai.azure.com/`).  
   - Copy an **API Key** (Key 1 or Key 2) for authentication.

2. **Microsoft 365 / SharePoint Online**  
   - A SharePoint site with a **list** or **document library** to store generated sheets (we’ll reference these via Environment Variables).  
   - The list/library must have these columns:  
   - **Title** (Title) – Single line of text  
   - **Specifications** (Specifications) – Single line of text    
   - **Short Description** (ShortDescription) –Single line of text    
   - **Long Description** (LongDescription) – Multiple lines of text (Plain)  
   - **SEO keywords**  (SEOkeywords) – Single line of text  
   - **Generation Date** (GenerationDate) – Date only  
   - Ensure the Power Automate connection account has **Contribute** permissions on that list/library.

3. **Power Platform Environment**  
   - **Power Apps Premium** and **Power Automate Premium** licenses (required for the Azure OpenAI connector).  
   - A Power Platform environment in the **same Azure region** as your Azure OpenAI resource to avoid connectivity issues.

---

## Installation & Deployment

### 1. Configure Azure OpenAI

1. In the **Azure Portal**, open your **Azure OpenAI** resource.  
2. Under **Manage**, click **Deployments**.  
3. Verify your model (e.g., `gpt4`) is deployed and status is **Succeeded**. Copy the **Deployment ID** exactly.  
4. Under **Keys & Endpoint**, copy:  
   - **Endpoint** (e.g., `https://<your-resource>.openai.azure.com/`)  
   - **Key 1** (or Key 2) for use in Power Automate.
   - 
![image](https://github.com/user-attachments/assets/32c09738-977b-4f34-a1f6-f6ff27816c49)

![image](https://github.com/user-attachments/assets/41768b2e-c310-4540-aaf1-a2be9643a451)


### 2. Prepare SharePoint

1. Navigate to your SharePoint site (e.g., `https://contoso.sharepoint.com/sites/Marketing`).  
2. Create a new **list** or **library** named `ProductSheets` (or any name—this will be stored in an Environment Variable).  
3. Add or confirm these columns:  
   - **Title** (Title) – Single line of text  
   - **Specifications** (Specifications) – Single line of text    
   - **Short Description** (ShortDescription) –Single line of text    
   - **Long Description** (LongDescription) – Multiple lines of text (Plain)  
   - **SEO keywords**  (SEOkeywords) – Single line of text  
   - **Generation Date** (GenerationDate) – Date only  



### 3. Import the Solution into Power Platform

1. In **Power Apps Maker Portal** (https://make.powerapps.com), switch to your target environment.  
2. Click **Solutions** on the left nav, then **Import**.  
3. Select the **`Product Sheet Generator`** solution package (e.g., a `.zip` or `.msapp` file if provided).  
4. Follow the import wizard. Once imported, you’ll see a solution named **Product Sheet Generator**, containing:  
   - The **Power Apps Canvas App** (`App Genetate Product Descriptions`).  
   - The **Power Automate Flow** (`Generate_Product_Sheet_WF`).  
   - Two **Environment Variables** (for SharePoint):  
     1. `SPSite` – stores the SharePoint site URL (e.g., `/sites/Marketing`).  
     2. `SPListeProducts` – stores the list or library name (e.g., `ProductSheets`).  

5. Click **Publish All Customizations** to finalize the import.

---

### 4. Configure Environment Variables

After importing, you must set the values of the two environment variables so the flow knows where to save generated sheets:

1. In **Power Apps Maker Portal**, go to **Solutions → Product Sheet Generator → Environment Variables**.  
2. Edit `SPSite`:  
   - Set its **Value** to the relative URL of your SharePoint site (e.g., `/sites/Marketing`).  
3. Edit `SPListeProducts`:  
   - Set its **Value** to the exact name of your list or library (e.g., `ProductSheets`).  
4. Save each environment variable.


### 5.The Power Apps Canvas App
![image](https://github.com/user-attachments/assets/3a7a1a7f-54e9-4240-9aeb-77c529706676)


After adding the product title and specification in the New form: 
1. Select the **Generate Sheet** button to generate the short and long descriptions and the SEO words.  
2. Select the **Validate** button to copy informations in the fill the New form.
3. Select **Save** button to save the new item in the SharePoint list

   `
