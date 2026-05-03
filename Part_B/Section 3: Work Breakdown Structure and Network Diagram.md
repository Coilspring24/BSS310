### Section 3: Work Breakdown Structure and Network Diagram

#### 3.1 Work Breakdown Structure (WBS)

The production process is divided into three distinct phases to ensure systematic execution and quality control.

* **Phase 1: Start-up Phase (Facility Preparation and Tooling)**
  * 1.1 Procure manufacturing equipment (Soldering stations, multimeters).
  * 1.2 Set up the 36m² decentralised manufacturing cell (Workbenches, ESD mats).
  * 1.3 Configure Raspberry Pi automated testing rigs.
* **Phase 2: Component Assembly (Integration)**
  * 2.1 Receive and serialize raw component stock (ESP32, LoRa modules).
  * 2.2 Execute precision through-hole soldering for 49 standard nodes.
  * 2.3 Execute precision soldering for the central gateway.
  * 2.4 Flash firmware to all MCU units.
* **Phase 3: Environmental Hardening and Validation**
  * 3.1 3D print PETG internal support sleds.
  * 3.2 Pot capacitive moisture sensors with protective epoxy.
  * 3.3 Perform final multimeter validation and seal IP67 enclosures.

#### 3.2 Network Diagram Data (Start-up Phase Only)

The following tasks represent the "Start-up Phase" and are the only tasks plotted on the network diagram. 

*(**Note for final submission:** A physical node diagram must be drawn using this data, ensuring a single start and end node, and clearly indicating the Critical Path, Forward Pass, Backward Pass, and Float inside each node).*

| Task ID | Task Description                                    | Predecessor | Duration (Days) |
|:------- |:--------------------------------------------------- |:----------- |:--------------- |
| **A**   | Project Start                                       | -           | 0               |
| **B**   | Secure 36m² facility (Garage clearing)              | A           | 2               |
| **C**   | Procure workbenches and ESD mats                    | B           | 3               |
| **D**   | Procure electronic tooling (Soldering, Multimeters) | B           | 4               |
| **E**   | Procure Raspberry Pi and configure testing rig      | D           | 2               |
| **F**   | Install equipment and set up assembly lines         | C, E        | 2               |
| **G**   | Calibrate tools and run test assembly               | F           | 1               |
| **H**   | Start-up Phase Complete (End Node)                  | G           | 0               |

**Critical Path Analysis:**
The critical path follows **A → B → D → E → F → G → H**, resulting in a total duration of 11 days. Task C contains exactly 1 day of float.







Based on the strict BSS 310 rubric and the "Common Mistakes" document we reviewed earlier, **Section 3** is where you prove you know how to plan and schedule a project using Operations Management techniques.

It consists of two specific parts: the **Work Breakdown Structure (WBS)** and the **Network Diagram**.

Here is exactly what you are supposed to do to get full marks:

### 1. The Work Breakdown Structure (WBS)

Think of the WBS as a glorified to-do list. You need to break down your entire production process (from setting up the garage to sealing the final box) into logical steps.

**The Strict Rules you must follow:**

- **Three Phases Minimum:** You must divide your work into at least three phases.

- **The "Start-up" Rule:** Phase 1 *must* be explicitly named the "start-up" phase. This phase covers everything you do *before* you start building the actual 49 nodes (e.g., clearing the garage, buying soldering irons, setting up the Pi tester).

- **Coherence:** Your WBS phases must logically flow and make sense for your specific Production portfolio.

### 2. The Network Diagram

This is a visual flowchart (using nodes and arrows) that maps out the timeline of your tasks to find out how long the project will take.

**The Strict Rules you must follow (where most students lose marks):**

- **Only Phase 1:** You must *only* draw the network diagram for the tasks in your "start-up" phase. Do not put Phase 2 or 3 on this diagram.

- **Single Start/End:** The diagram must start with exactly one "Start" node and finish with exactly one "End" node.

- **Side Connections Only:** The arrows connecting your tasks must enter and exit from the *sides* of the nodes. If an arrow touches the top or bottom of a box, you lose marks.

- **The Mandatory Math:** Inside every single node, you must calculate and visually write down the **Forward Pass**, **Backward Pass**, and **Float (Slack)**.

- **The Critical Path:** You must clearly highlight or indicate the Critical Path (the sequence of tasks that have zero float).

### How this ties into the draft I gave you:

In the markdown draft I provided previously, I wrote out the WBS (meeting the 3-phase rule) and gave you a table of data for the Start-up Phase.

**Your actual job for Section 3 now is to physically draw that network diagram** (usually in draw.io, Visio, or PowerPoint) using the table data I provided, making sure to do the math (Forward/Backward passes) and follow the layout rules above, and then insert that image into your report.

Does the math part (Forward/Backward pass and Float) make sense, or do you want me to map out exactly what numbers need to go in each box for your specific diagram?


