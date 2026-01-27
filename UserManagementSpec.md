
# User Management Module - UI/UX Specifications

## 1. Page Layout & Structure
The screen follows a **Master-Detail pattern** split into two main panels:
* **Left Panel (DataGrid):** Displays the collection of existing users.
* **Right Panel (Edit Form):** Contains the input controls for creating or editing the `SelectedItem`.
* **Top Bar:** Contains global filters and action buttons (Toolbar).

---

## 2. Functional Requirements

### 2.1. Top Action Toolbar
* **"+ New User" Button**
    * **Event:** `Click` event resets the *Right Panel* controls to default values.
    * **State:** Sets `SelectedItem` in the DataGrid to `null`.
    * **Style:** Primary Action Style.
* **"Hide Disabled User" CheckBox**
    * **Property:** `IsChecked` defaults to `true`.
    * **Behavior:** When `CheckChanged`, triggers a LINQ query or filter on the `DataSource` (`Where(u => u.Enabled == true)`). Triggers `DataGrid.Refresh()`.
* **"Save User" Button**
    * **Event:** Triggers `Submit` logic.
    * **Logic:**
        * If **Mode == Create**: Calls `POST` API endpoint.
        * If **Mode == Edit**: Calls `PUT/PATCH` API endpoint.
    * **Validation:** Checks `ModelState` or validation rules; blocks submission if invalid.

### 2.2. Left Panel: User DataGrid
**Columns (Properties):**
1.  **ID:** `int`, Sortable.
2.  **UserName:** `string`, Sortable, Filterable.
3.  **Email:** `string`, Sortable, Filterable.
4.  **Enabled:** `bool` (True/False), Sortable.

**Grid Interactions:**
* **Sorting:** Column headers trigger `OrderBy` / `OrderByDescending`.
* **Filtering:** Header filters apply `Contains()` search on the `ICollectionView` or List source.
* **Selection:**
    * `SelectionChanged` event populates the *Right Panel* controls with the selected object's properties (Data Binding).

### 2.3. Right Panel: User Form
**Header:** Dynamic Label. Displays "New User" or "Edit User: {UserName}".

**Form Controls:**

| Label | Control Type | Data Type | Validation / Behavior |
| :--- | :--- | :--- | :--- |
| **Username** | `TextBox` | `string` |  Check for uniqueness via API. |
| **Display Name** | `TextBox` | `string` | Optional. `MaxLength` = 100. |
| **Phone** | `TextBox` | `string` | Input Mask or Regex validation for phone format. |
| **Email** | `TextBox` | `string` |  Validate using `EmailAddressAttribute` or Regex. |
| **User Roles** | `ComboBox` (MultiSelect) | `List<Role>` |<br> **DataSource:** Guest, Admin, SuperAdmin. <br> *UI Behavior:* Binds to a `SelectedRoles` collection. |
| **Enabled** | `CheckBox` | `bool` | Default `IsChecked` = `false` for new records. |

---

## 3. UI/Visual Guidelines
* **Color Palette:**
    * Primary/Action: Dark Blue (`#005A9E`).
    * Success/Save: Info Blue (`#17A2B8`).
    * Selection: Light Blue (`#D1ECF1`).
* **Icons:** Use standard generic icons (e.g., FontAwesome or Segoe UI Symbol).
* **Feedback:**
    * **Success:** Show a Notification/Toast ("User saved successfully") on `await Task` completion.
    * **Error:** Bind error messages to `ErrorProvider` or display inline validation text (Red).