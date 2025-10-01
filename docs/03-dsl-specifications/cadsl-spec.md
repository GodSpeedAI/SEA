# **Cognitive Artifact DSL (CADSL) Specifications**

## **1. Introduction**

The Cognitive Artifact DSL (CADSL) is a declarative language for defining and constructing dynamic, interactive cognitive artifacts within the Sentient Enterprise Architecture (SEA). It is designed to be both human-readable and machine-interpretable, enabling AI agents to generate and recommend artifacts in real-time to support knowledge work. The CADSL is built upon a MECE (Mutually Exclusive and Collectively Exhaustive) set of symbolic elements that can be combined to create a wide variety of cognitive artifacts, from simple checklists to complex, interactive diagrams.

## **2. Core Principles**

*   **Modularity and Composability:** Artifacts are composed of a small set of fundamental, reusable elements.
*   **Declarative Syntax:** The DSL defines *what* an artifact is, not *how* to render it. This separates the artifact's structure and content from its visual presentation.
*   **Dynamic and Interactive:** Artifacts are not static. They can be edited by the user, and their state can change in response to user interaction or external events.
*   **Context-Aware:** The generation and content of artifacts are informed by the conversational context, user needs, and the broader knowledge base of the SEA.

## **3. MECE List of Symbolic Elements**

The CADSL is composed of the following MECE set of symbolic elements, which can be combined to create any cognitive artifact:

### **3.1. Content Elements**

*   **Textual Content:**
    *   `TextField`: Single-line text input.
    *   `TextArea`: Multi-line text input.
    *   `Label`: Descriptive text for other elements.
    *   `Heading`: Main title or section heading.
    *   `Subheading`: Secondary title within a section.
    *   `Paragraph`: Block of text content.
*   **Visual Content:**
    *   `Image`: Picture or illustration.
    *   `Icon`: Symbolic representation.
    *   `Diagram`: Visual representation of concepts.
    *   `Chart`: Data visualization.

### **3.2. Structural Elements**

*   **Containers:**
    *   `Section`: Divides content into parts.
    *   `Panel`: Separate area within a workspace.
    *   `Frame`: Isolated view or context.
    *   `Tab`: Navigable content area.
    *   `Canvas`: Area for creating and organizing content.
*   **Lists and Grids:**
    *   `OrderedList`: Sequenced items.
    *   `UnorderedList`: Non-sequenced items.
    *   `Table`: Structured data representation.

### **3.3. Interactive Elements**

*   **Input Controls:**
    *   `Button`: Clickable action.
    *   `Checkbox`: Binary option.
    *   `RadioButton`: Mutually exclusive selection.
    *   `Dropdown`: Selectable options from a list.
    *   `Slider`: Adjustable value within a range.
    *   `Toggle`: On/off state.
    *   `SelectionGroup`: Grouped selectable items.
*   **Interactive Regions:**
    *   `DragAndDropArea`: Region for moving or rearranging items.
    *   `ClickableRegion`: Area that responds to user clicks.
    *   `ItemSelector`: Tool for choosing items (e.g., date picker).

### **3.4. Relational Elements**

*   **Connectors:**
    *   `Line`: Indicates a relationship or flow.
    *   `Arrow`: Indicates directionality.
*   **Links:**
    *   `Hyperlink`: Reference to an external or internal resource.
    *   `Reference`: Citation or pointer to other content.

### **3.5. Organizational Elements**

*   **Hierarchy Indicators:**
    *   `Indentation`: Shows parent-child relationships.
    *   `Nesting`: Elements within elements.
*   **Grouping Mechanisms:**
    *   `Folder`: Organizes files or content.
    *   `GroupBox`: Visually groups items.

### **3.6. Annotation Elements**

*   **Notes and Comments:**
    *   `StickyNote`: Small text note attached to content.
    *   `CommentBubble`: Annotation linked to a specific part.
*   **Highlighting and Marking:**
    *   `Highlight`: Emphasizes text or areas.
    *   `Underline`: Indicates importance or links.
    *   `Flag`: Indicator for attention or action.

### **3.7. Temporal Elements**

*   **Time Indicators:**
    *   `Timeline`: Chronological representation of events.
    *   `Calendar`: Dates and scheduling.
    *   `Schedule`: Planned activities or tasks.
    *   `ProgressBar`: Visualizes completion status.

### **3.8. Feedback Elements**

*   **Status Indicators:**
    *   `Notification`: Alert for an event or update.
    *   `Alert`: Urgent or important message.
    *   `Badge`: Counter or status symbol.
    *   `ProgressIndicator`: Shows an ongoing operation.

### **3.9. Semantic Elements**

*   **Tags and Metadata:**
    *   `Tag`: Descriptive label for searching or grouping.
    *   `CategoryLabel`: Defines content type.
    *   `MetadataField`: Additional information about content.
    *   `Legend`: Explanation for symbols or colors.

### **3.10. Collaboration Elements**

*   **User Indicators:**
    *   `Avatar`: Visual representation of a user.
    *   `PresenceIndicator`: Shows active participants.
*   **Shared Components:**
    *   `SharedEditingArea`: Collaborative workspace.
    *   `UserComment`: Feedback and discussion from other users.

## **4. Example CADSL Definition**

Here is an example of how a simple project planner cognitive artifact could be defined in the CADSL (represented here in YAML for readability):

```yaml
artifact: project-planner
canvas:
  - type: Heading
    content: "Project Plan: Launch New Feature"
  - type: Section
    id: objectives
    children:
      - type: Subheading
        content: "Objectives"
      - type: UnorderedList
        items:
          - "Increase user engagement by 15%"
          - "Improve feature discovery"
  - type: Section
    id: tasks
    children:
      - type: Subheading
        content: "Tasks"
      - type: OrderedList
        items:
          - type: Checkbox
            label: "Design new UI mockups"
            checked: false
          - type: Checkbox
            label: "Develop backend API"
            checked: false
          - type: Checkbox
            label: "Implement frontend components"
            checked: false
  - type: Section
    id: timeline
    children:
      - type: Subheading
        content: "Timeline"
      - type: Calendar
        events:
          - date: "2025-11-15"
            label: "Design complete"
          - date: "2025-12-01"
            label: "Development complete"
```

This CADSL definition would then be interpreted by a rendering engine to display the interactive project planner to the user. The AI agent can dynamically modify this structure based on the ongoing conversation, adding new tasks, updating objectives, or adjusting the timeline as needed.
