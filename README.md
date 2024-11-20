## Uncia Application Overview

Uncia is an application designed for professionals in the IT departments of companies. Its primary goal is to assist in the creation of technical documentation—referred to as "Projects" within the platform. By enabling users to establish relationships between technical documents, Uncia highlights the connections between different IT services or business applications documented within it. The ultimate objective is to generate a comprehensive cartography of the Information System, known as the **Information System Map**.

---

## Technical Implementation

Uncia is a multi-tenant application, where each tenant is dedicated to an organization representing a company's IT department. This structure ensures that each organization has its own secure and isolated environment within Uncia.

---

## Configuration by the Organization Owner

Before producing documents, Uncia must be configured by the **Organization Owner**. The configuration includes defining user roles, setting up connections, establishing rules, and more.

### 1. Defining User Roles

- **Organization Owners**: Admins with full permissions over the organization's Uncia environment.
- **Users**: By default, have read-only access to all projects.
  - **Project Owner**: A user who creates a project; has full permissions on that project, including deletion rights.
  - **Editor**: A user granted edit privileges on a project by the Project Owner.
  - **Reviewer**: A user assigned to review specific project versions; projects can contain multiple versions.
- **Deployment Team**: Users who do not have direct access to the application but receive email notifications to build the content of a project version. They have token access to the API.

### 2. Connecting to Azure Tenant

Uncia can connect to an Azure tenant using service principal credentials. This integration enables Uncia to interact with Azure services and resources.

### 3. Setting Rules

The Organization Owner can define various rules to guide project creation and ensure compliance:

#### a. Network Zones and Flows

- **Network Zones**: Define the network zones of the environment to describe its urbanization.
- **Forbidden Flows**: Specify prohibited protocols and flows between network zones.

#### b. Object Parameters for Architecture Drawing

Parameters necessary for drawing the architecture (using drag-and-drop with React Flow nodes and edges):

- **Network Zone Parameters** (e.g., subnet)
- **Simple Internal Module**: Logical module describing a specific function of the application.
- **Complex Internal Module**: Logical module with multiple bricks, representing sub-functions.
- **Simple and Complex External Module**: Similar to internal modules but external to the application. They can be:
  - Generic services consumed by multiple projects.
  - Services specific to a project.
  - Other internal modules exposed to other projects with particular rules.
- **Internal and External Bricks**: Components included in complex modules.
- **User**: Represents an end-user or user group.
- **Cluster of Network Zones**: Internal or external clusters to apply principles to multiple network zones (less commonly used).
- **Flow Edge**: Represents interactions between modules.

#### c. Naming Convention

Define conventions to automatically generate object IDs during drawing and for infrastructure asset templates, enabling automatic reference generation.

#### d. Workflow Parameters for Review Cycle

Uncia incorporates a review process to ensure that project versions are approved by designated reviewers.

- **Review Process Steps**:
  1. **Draft**: Creation phase by the Project Owner and Editors.
  2. **In Review**: Reviewers assess, comment, and approve or reject the version. Project Owners or Editors can modify the document based on feedback.
  3. **Approved**: Validated by all reviewers or forced by the Organization Owner if rejected by any reviewer; editing is locked.
  4. **Design to Build**: The project version's content becomes available via API. Deployment Team members receive instructions to deploy. Uncia detects deployed resources in the subscription (currently limited to Azure).
  5. **Design to Build Alignment**: Uncia checks that infrastructure assets and attributes are correctly deployed in Azure. Discrepancies generate a conformity report. Project Owners or Editors can accept changes or instruct the Deployment Team to adjust.
  6. **Deployed**: The design and subscription content are perfectly aligned; editing is locked.

- **Modifiable Workflow Parameters**:
  - **Number of Rounds**: A round includes the reviewer's feedback and subsequent modifications by the Project Owner or Editor. Reviewers can approve or reject after all rounds or sooner.
  - **Round Duration**: Time allocated for reviewers and editors to complete a round.
  - **Automatic Validation**: Option to automatically validate if all reviewers approve.
  - **Notification Frequency**: How often notifications are sent to reviewers and editors.
  - **Flags**: Reviewers can set flags on comments (e.g., blocking or non-blocking for approval).

#### e. CIAT Parameters

Define scales (e.g., 1 to 4) for:

- **Confidentiality**
- **Integrity**
- **Availability**
- **Traceability**

These parameters influence project properties during creation or editing.

### 4. Notification Center

Displays review process notifications for any user or Organization Owner involved in the review process.

### 5. Infrastructure Asset Setup

Users can set up templates of infrastructure assets representing virtual, hardware, or third-party components (currently, only Azure integration is available).

- **Creation of Infrastructure Assets**:
  - Select the type and attributes (defined by the Organization Owner).
  - Attributes can include other infrastructure assets, allowing for linked dependencies.
  - By default, an infrastructure asset includes:
    - **Reference**: Inherits from the naming convention if defined.
    - **Environment**: e.g., development, pre-production, production.
  - An infrastructure asset is defined by a name and multiple attributes.

- **Third-Party Integration**:
  - Link infrastructure asset templates with Azure Resource Management (ARM) templates (e.g., `Microsoft.Compute/VirtualMachines` for an Azure VM).
  - Link attributes with Azure resource properties by defining the path in the ARM resource JSON structure.
  - Linked resources are pulled into the Cloud conformity report.

### 6. Product List

Define products representing technologies used by the infrastructure.

- **Product Definition**:
  - **Attributes**: Name, family (e.g., software, middleware, OS), editor, license model.
  - **Versions**: Add and manage versions, setting end-of-support and end-of-life dates.
- **Product Usage**:
  - Products serve as specific types for infrastructure asset attributes.
  - Versions can be marked as allowed, recommended, or forbidden, influencing project configurations.
  - Define compatibility rules between products and versions.

### 7. Information System Mapping (ISMapping)

Provides several functionalities:

#### a. Interconnections and Dependencies

- Displays interconnections and dependencies between applications (e.g., internal module to external module).
- Graphically represents these relationships.

#### b. Infrastructure Asset Listing

- Lists all infrastructure assets from approved project versions.
- Helps correlate assets with projects, modules, products, and lifecycle information.

#### c. Intermediate Objects

- **Definition**: Network or security objects between a source and destination (e.g., module to user).
- **Purpose**: Capture flow requirements per project.
- **Usage**: Can be used in project versions.

#### d. External Services Management

- Define and manage generic external services.
- An external generic service can be associated with multiple network zones.

#### e. Global IS Mapping View

- Represents all interconnections and dependencies across projects and services.

### 8. Document Synthesis Template

Create a document skeleton with sections and subsections, defining expected content using various data representations (tables, bullet points, images, checklists).

- **Inheritance**: All projects inherit from this template.
- **Versioning**: Changes to the template do not affect existing project versions; only new versions inherit updates.
- **Required Sections**: Sections can be marked as required, blocking submission for review until completed.
- **Limitations**: No mechanism prevents sections from being removed when a project version inherits from the latest template.

---

## Definition of a Project

A project in Uncia is defined by:

### Basic Information

- **Name**
- **Trigram**: An abbreviation.
- **Description**
- **CIAT Parameters**: Confidentiality, Integrity, Availability, Traceability scales.
- **RTO**: Recovery Time Objective.
- **RPO**: Recovery Point Objective.
- **BCP Parameters**: Business Continuity Planning.
- **Template Inheritance or Import**: Option to inherit from a template or import from a flow matrix.
- **User Permissions**

### Project Versions

- **Multiple Versions**: A project can have several versions.
- **Version Creation**: To create a new version (minor, intermediate, major), the previous one must be approved or deployed.
- **Reviewers and Deployment Team**: Selected at submission for review for the first version and at creation for subsequent ones.

---

## Drawing and Editing

When a project version is opened, the first view is the diagram panel, based on React Flow. Users can drag and drop objects onto the drawing area.

### Objects

- **Nodes**: Each object (node) includes pinpoints configurable via a right panel.
- **Internal Modules/Bricks**: Represented with a plain color background; colors and icons are customizable.
- **External Modules/Bricks**: Represented with a hatched color background; colors and icons are customizable.
- **Icon Bank**: Includes Azure, AWS, Google Cloud, and Material icons.

### Defining External Modules

- **As an External Service**: Simply name the module.
- **Link to a Generic Module**:
  - Define directly in the module configuration panel.
  - Select from a list of generic modules defined in other projects or the ISMapping section.
- **Best Practice**: Place modules inside network zones for clarity and adherence to IT architecture standards.

### Flows and Intermediate Objects

#### Flows

- **Types**:
  - **User Flows**: From a user.
  - **Internal Flows**: Between internal modules.
  - **External Flows**: Toward an external module.
- **Drawing**: Represent interactions between objects.

#### Intermediate Objects

- **Function**: Placed between flows to represent network/security components.
- **Requirements**:
  - Must receive an incoming arrow with a defined flow before an outgoing arrow can be drawn.
  - Arrows can contain multiple flows with specified protocols and optional ports.
- **Management**:
  - Each has a dedicated tab in the flow cartography for flow management.
  - Can be dragged multiple times for representation flexibility.
  - Security measures prevent drawing flows between the same intermediate object.
- **Flow Integrity**: If a flow is removed, a warning appears to indicate a broken chain.
- **Export**: Flows can be exported in Excel format.

### Dependencies

- **Creation**: Drawing a flow between an internal module/user and an external module creates a dependency.
- **Logging**: Dependencies are logged and represented graphically.
- **Visualization**: Clicking on nodes in the dependency graph expands relationships.

### Forbidden Flows

- **Detection**: Flows matching forbidden rules generate a popup warning.
- **Reporting**: Adds an entry in the conformity report's flow section.

### Infrastructure Assets in Modules

- **Association**: Internal modules/bricks can have multiple infrastructure assets.
- **Management**:
  - Managed in the module settings panel.
  - Select infrastructure templates and specify attribute values.
  - References are editable if no naming convention is defined.
- **Reusability**:
  - Existing infrastructure assets from approved or deployed versions can be reused.
  - Assets can be reused within different modules or bricks in the same project version.

### Infrastructure Asset Views

- **Infrastructure Table**: An entry is added when an asset is created.
- **Templates Tab**: Lists all infrastructure assets by template.
- **Custom Views**: Represent assets based on common attributes; customizable.
- **Compliance**:
  - Violations (e.g., forbidden product versions) generate a popup and an entry in the conformity report's product section.

### Modifying the Synthesis Document

- **Flexibility**: Each section of the synthesis document can be modified as needed.

---

## Review Process

### Submission for Review

- **Initiation**: Project Owners and Editors submit a project version for review.
- **Notifications**: Reviewers receive notifications in Uncia and via email.
- **Process**: Reviewers assess, comment, and can approve or reject the version.

### Deployment Process

- **Post-Approval**: If deployment is required (currently Azure only), upon approval:
  - Deployment Team members receive notifications to access the project version's API endpoint.
  - First-time recipients get an authentication token via email.
- **Deployment**: The team deploys according to the provided JSON output.

### Linking to Azure Subscription

- **Popup Prompt**: Appears to link the project version with an Azure subscription.
- **Requirements**:
  - **Tenant ID**
  - **Service Principal Credentials**
  - **Subscription Name**
- **Check Frequency**: Users specify how often Uncia checks Azure (e.g., per minute, hour).

### Cloud Conformity Report

- **Access**: Project Owners/Editors can consult the report after the specified interval.
- **Sections**:
  - **Build Alignment**: Shows discrepancies between the project and Azure resources.
  - **Logs**: Records ongoing changes and issues.
- **Alignment Process**:
  - **Discrepancies**: Must be resolved for the version to be considered deployed.
  - **Actions**:
    - **Resource in Azure but not in Uncia**:
      - **Reject**: Deployment Team instructed to remove the resource.
      - **Accept**: Link the resource to an infrastructure asset in Uncia.
    - **Resource in Uncia but not in Azure**:
      - **Reject**: Deployment Team instructed to add the resource.
      - **Accept**: Remove the infrastructure asset from Uncia.
    - **Attribute Mismatches**:
      - **Accept**: Update Uncia to match Azure.
      - **Reject**: Deployment Team instructed to correct Azure properties.
- **Completion**: All discrepancies must be resolved for deployment.
- **Ongoing Monitoring**: Logs continue to capture changes that could cause issues.

---

**Definition of a Template**

Templates in Uncia are akin to projects and follow similar principles, including versioning up to the approved status (the last three review steps are excluded). To be utilized in a project, a template must be manually approved by a Reviewer or the Organization Owner. There are no content restrictions on templates.
