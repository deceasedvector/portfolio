---
title: Standard Operating Procedure
---

# Initiating bulk email invoices for UPS

__Standard Operating Procedure__

Department | Version | Publish date
---|---|---
Business Intelligence | 1.0 | 5/3/2024

## Purpose

This document details the process of initiating a bulk send of invoice PDFs to UPS. 

After Accounts Receivable have changed the Sales Orders' statuses to "Ready to Invoice", the AR Manager will create a case requesting the bulk invoices be sent, including with the applicable date range.

=== "Access"

    - [x] NetSuite *Waste Harmonics Administrator Jr* role.

=== "Resources"

    - [Billing Customers](#)
    - [Saved Searches](#)
    - [Workflows](#)

## Procedure

### 1. Update saved search

1. In the NetSuite **Global Search Bar**, enter "*sea: ups email send*" and select **Edit** on the suggestion.
2. Under **Criteria**, update the **Date** filter to the requested date range.
3. Optionally, update any other fields.
4. Select **Save**.

### 2. Update the workflow

1. Go to **Customization** > **Workflow** > **Workflows**.
2. Find the *UPS Invoice Send* workflow and select **Edit**.
3. In the **Workflow** sidebar, select **Edit**. 

    The Workflow window opens.

4. Under **Schedule**:

    1. For **SAVED SEARCH FILTER**, choose *UPS Email Send*.
    2. For **EXECUTION DATE**, choose the current date.
    3. For **EXECUTION TIME**, choose a time in the afternoon for best results.

5. If any other fields were requested, update them as needed.
6. Select **Save**.

### 3. Update the workflow state

1. Under **Workspace**, select the **State 1** box.
2. In the **State** sidebar, select **Edit**.

    The Workflow State window opens.

3. Under **Actions**, select **Send Email**.

    The Workflow Action window opens.

4. Under **Schedule**, for **START TIME**, match the time you selected for **EXECUTION TIME** on the workflow.
5. Under **Sender**:

    1. Select the **SPECIFIC SENDER** checkbox.
    2. For **SENDER**, link the Employee Record from whom the emails will send.

6. Under **Receipient**, in **EMAIL**, enter the receiving customer's email address.
7. Under **Content**:

    1. Select the **CUSTOM** checkbox.
    2. In **SUBJECT**, enter the subject template.

        ???+ note
            Some customers have specific formats the SUBJECT must adhere to.
    
    3. In **BODY**, enter any applicable information.

8. Under **Attachment**:

    1. Select the **INCLUDE TRANSACTION** checkbox.
    2. For **TYPE**, choose *PDF*.

9. Select **Save** on the workflow action.
10. Select **Save** on the workflow state.
11. Wait for the workflow to run.

### 4. Verify the run

1. Go to the customer's page.
2. Go to **Sales** > **Transactions** and open an invoice.
3. On the invoice, go to **Communication** > **Messages**. If successful, you'll see the generated email.