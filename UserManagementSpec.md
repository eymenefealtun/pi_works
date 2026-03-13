# UI Specification Document: User Management Screen

## 1. Overview
This document outlines the user interface requirements, component details, and expected behaviors for the "User Management" screen. This interface allows administrators to view a list of existing users, filter the list, and add new users or edit existing ones.

## 2. Page Layout
The screen is divided into three main sections:
1.  **Top Action Bar:** Contains global actions and filters.
2.  **Left Pane (Data Grid):** Displays the list of users in a tabular format.
3.  **Right Pane (Details Form):** A form used for creating a new user or displaying/editing the details of a selected user.

## 3. Initial State (On Page Load)
* **Data Grid:** Populated with the initial fetch of user data.
* **"Hide Disabled User" Checkbox:** Checked by default. The Data Grid should only display active users (`Enabled = true`).
* **Right Pane Form:** Empty and set to "New User" mode by default (or disabled until the `+ New User` button is clicked or a row is selected).
* **"Save User" Button:** Disabled or inactive until valid data is entered into the form.

---

## 4. UI Components & Behaviors

### 4.1. Top Action Bar
| Component | Type | Behavior / Action |
| :--- | :--- | :--- |
| **`+ New User` Button** | Primary Button (Blue) | Clears all fields in the Right Pane form. Changes the form title to "New User". Focuses on the first input field ("Username"). |
| **`Hide Disabled User`** | Checkbox | **Checked (Default):** Filters the Data Grid to hide users where the `Enabled` status is false. <br> **Unchecked:** Displays all users regardless of their status. |
| **`Save User` Button** | Primary Button (Blue) | Triggers form validation. If valid, submits the data (POST for new user, PUT/PATCH for existing user) and refreshes the Data Grid to reflect the changes. |

### 4.2. Left Pane: User List (Data Grid)
A responsive table displaying user records. Row selection is supported.

* **Columns:**
    * **ID:** Numeric identifier.
    * **User Name:** Alphanumeric username.
    * **Email:** User's email address.
    * **Enabled:** Boolean status (displayed as `true` or `false`).
* **Column Features:** Every column header includes sorting (up/down arrows) and filtering (funnel icon) capabilities.
* **Row Interaction:** Clicking a row selects it (indicated by a background color highlight, e.g., light blue for row ID 2). Upon selection, the Right Pane form is populated with the selected user's details, and the form title should ideally change to "Edit User".

### 4.3. Right Pane: Details Form
This section dynamically handles both user creation and updates.

* **Title:** Displays "New User" when creating a user, or "Edit User" (implied) when an existing user is selected.
* **Form Fields:**
    1.  **Username:** Text input field. (Required)
    2.  **Display Name:** Text input field. (Required)
    3.  **Phone:** Text input field. Accepts numeric/phone formats.
    4.  **Email:** Text input field. Must include email format validation (`*@*.*`). (Required)
    5.  **User Roles:** Multi-select dropdown. Displays available roles. 
        * *Available Options:* Guest, Admin, SuperAdmin.
        * *Placeholder:* "Select user roles..."
    6.  **Enabled:** Checkbox. Sets the user's active status. (Checked = `true`, Unchecked = `false`).

## 5. Validation Rules & Error Handling
* **Required Fields:** If the user attempts to click "Save User" while required fields (Username, Display Name, Email) are empty, display inline validation errors (e.g., red border around the input and a small error message below it).
* **Email Format:** The system must verify standard email formatting before submission.
* **Success State:** Upon successful save, display a temporary success toast notification (e.g., "User saved successfully") and clear/refresh the UI as needed.
