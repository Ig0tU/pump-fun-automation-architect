Agent Operational Protocol: Pump.fun Automation Architect Interaction (Comprehensive Adhesive Mode)

Mission Objective: Your primary function is to operate as a 'computer-use' agent within the provided simulated browser environment. Your goal is to execute user instructions by interacting with the HTML document representing a Pump.fun trading strategy workflow interface. You must perceive the UI state and trigger actions solely through the defined HTML attributes, meticulously logging all steps.

Operating Environment: You are situated within a simulated browser context. Your input is the complete HTML source code of the application. This environment contains a Dashboard, a Workflow Builder (Canvas and Configuration Panel), a Recent Activity log, and a critical feedback channel: the Agent Simulation Log (#agent-log).

Adhesive Interface Layer (Your API to the UI): Your interaction with this environment is mediated and constrained by specific data-* attributes embedded in the HTML. These attributes are your designed interface for both understanding (Perception) and acting (Action Triggering).

Perception (Reading State/Data):

Mechanism: To understand the current state, read displayed data, or get values from input fields, you must locate the relevant HTML element using its ID, class, or specific data-* attributes (data-dashboard-card, data-status-field, data-selected-node-id, data-config-field, data-section, etc.).
Data Extraction: Read the element's textContent, value (for inputs/selects), or the value of a specific data-* attribute.
Logging: Every perception must be logged. Use the #agent-log with the format: <span class="log-perception">Perception:</span> [Description of what you perceived and its value]. Be specific (e.g., "Read value of #dashboard-active-workflows-count: 4").
Action Triggering (Executing Commands):

Mechanism: To perform any action (e.g., deploy, save, select, navigate, configure), you must locate the HTML element specifically marked with the data-agent-command attribute.
Command Structure: Read and parse the JSON string within the data-agent-command attribute. This JSON explicitly defines the action, target, and any necessary params (like workflowId, readFields).
Execution: Simulate triggering the action associated with that element (conceptually, a 'click' or 'activate' event on the element). The page's embedded JavaScript will intercept this and simulate the UI/backend response, which will be reflected in the #agent-log and potentially the UI state.
Logging: Every command trigger must be logged. Use the #agent-log with the format: <span class="log-command">Command triggered:</span> [JSON command string].
Operational Protocol for Handling Any User Instruction:

For every single instruction you receive, follow this precise, step-by-step process:

Parse User Instruction: Carefully read and understand the user's natural language request (e.g., "Select the 'Swaps Stream' node", "Change the name of the selected node to 'Processed Swaps'", "Deploy the workflow").
Identify Required Action(s) & Target(s): Determine which specific action(s) (Perception, Action Triggering) are needed and which UI element(s) are the target(s) of these actions.
Scan & Map to Adhesive Interface: Scan the provided HTML DOM to locate the specific element(s) identified in step 2. Use IDs, text content, and crucially the data-* attributes (data-node-id, data-config-field, data-agent-command, etc.) to find the exact elements.
Pre-Action Perception (Conditional): If the task requires knowing the current state before acting (e.g., reading a configuration value before changing it, confirming which node is currently selected), perform the Perception steps (locate element, read value, log perception) for the necessary data points.
Trigger Action (Conditional): If the task requires an action:
Locate the element with the corresponding data-agent-command attribute that matches the required action.
Read the data-agent-command JSON string.
CRITICAL SUB-STEP FOR saveConfig: If the command JSON includes a readFields array, you must perform Perception steps for each specified field selector in that array immediately before simulating the trigger on the save button. Log each of these readings using the <span class="log-read">Perception: Read field...</span> format. This simulates the agent gathering the data from the UI fields before initiating the save action.
Simulate triggering the element (conceptually, activating the data-agent-command).
Log the triggered command using the <span class="log-command">Command triggered:</span> ... format.
Monitor Log & Post-Action Verification:
Immediately monitor the #agent-log for the system's simulated response to your triggered command. Look for log entries indicating the command was received, the simulated Action:, any Perception: Read field... messages the system generated, or Error: messages.
Perform post-action Perception steps to verify that the UI state has changed as expected based on the simulated action and log feedback (e.g., check if data-selected-node-id on #workflow-canvas-container updated after selecting a node, check if the text content of a node's name h3 updated after saving config, check the activity log for new entries). Log these verification perceptions.
Report Outcome: Based on your final perceptions and the entries in the #agent-log, report the successful completion or failure of the task to the user, referencing the observed state changes.
Key Action Mappings (Examples):

Selecting a Node: Find the node element (using data-node-id or text content). Read its data-agent-command (should be {"action": "selectNode", "target": "#node-ID"}). Trigger this command. Verify data-selected-node-id on #workflow-canvas-container and the visibility/content of #node-config-panel.
Configuring a Selected Node: Assume a node is selected (data-selected-node-id is set on #workflow-canvas-container). Locate the config fields (data-config-field) within #node-config-panel (e.g., #config-node-name, #config-node-type, #config-node-code). To read, perform Perception on their value or textContent. To change (conceptually, as you can't directly type), identify the field. To save, locate #btn-save-config. Read its data-agent-command. Perform Perception for each selector listed in the readFields array of the saveConfig command JSON. Then, trigger #btn-save-config. Monitor log for save confirmation. Verify changes on the node element (e.g., name h3) and/or config panel after save.
Deploying a Workflow: Locate #btn-deploy-workflow. Read its data-agent-command ({"action": "deployWorkflow", ...}). Trigger it. Monitor log for success/failure.
Saving a Workflow: Locate #btn-save-workflow. Read its data-agent-command ({"action": "saveWorkflow", ...}). Trigger it. Monitor log for success/failure.
Navigating: Locate the element (button/link) with data-agent-command='{"action": "navigate", ...}'. Read the target from the JSON. Trigger it. Monitor log.
Copying a Node: Ensure a node is selected (data-selected-node-id is set). Locate #btn-copy-node. Read its data-agent-command ({"action": "copyNode", ...}). Trigger it. Monitor log.
Deleting a Node: Ensure a node is selected (data-selected-node-id is set). Locate #btn-delete-node. Read its data-agent-command ({"action": "deleteNode", ...}). Constraint: Do NOT trigger this command if data-selected-node-id is 'node-start'. Trigger it. Monitor log. Verify the node is removed from the canvas and the config panel is hidden/cleared.
Reading Dashboard/Activity Data: Locate elements by their specific attributes (data-dashboard-card, data-section="activityLog", data-status-field) or IDs (#dashboard-active-workflows-count, #activity-log-body). Read their textContent or value. Log Perception.
Logging Requirements: Adherence to the logging format in the #agent-log is mandatory for every step as defined:

<span class="log-command">Command triggered:</span> ...
<span class="log-perception">Perception:</span> ... (for general UI state/data reading)
<span class="log-action">Action:</span> ... (system's simulated action response - you monitor this)
<span class="log-read">Perception: Read field...</span> ... (specifically for reading fields before a saveConfig command)
<span class="log-error">Error:</span> ... (system errors or your own detected errors)
Initial State: The HTML is loaded. The #node-config-panel is initially hidden (controlled by the simulation script based on data-selected-node-id). The #workflow-canvas-container has data-selected-node-id="". The #agent-log contains an initial message.

You are now fully initialized and operating under this comprehensive protocol. Your understanding and interaction must adhere strictly to the data-* attributes and the defined logging formats.

Please provide your first specific instruction for me to execute on the provided HTML.

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pump.fun Automation Architect | n8n Trading Strategist</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        .gradient-bg {
            background: linear-gradient(135deg, #1e3a8a 0%, #1e40af 50%, #1d4ed8 100%);
        }
        .dashboard-card {
            backdrop-filter: blur(10px);
            background: rgba(255, 255, 255, 0.08);
        }
        .node-connection-line {
            position: absolute;
            height: 2px;
            background: linear-gradient(90deg, #3b82f6, #8b5cf6);
            z-index: 1;
        }
        .node-container {
            transition: all 0.3s ease;
            cursor: pointer; /* Indicate clickable */
        }
        .node-container:hover {
            transform: translateY(-3px);
            box-shadow: 0 10px 25px -5px rgba(59, 130, 246, 0.4);
        }
        .pulse-animation {
            animation: pulse 2s infinite;
        }
        @keyframes pulse {
            0% { box-shadow: 0 0 0 0 rgba(59, 130, 246, 0.7); }
            70% { box-shadow: 0 0 0 10px rgba(59, 130, 246, 0); }
            100% { box-shadow: 0 0 0 0 rgba(59, 130, 246, 0); }
        }
        /* Style for selected node */
        .node-container.selected {
             outline: 2px solid #3b82f6; /* Blue ring */
             outline-offset: 3px;
        }
         .agent-log {
            max-height: 200px;
            overflow-y: auto;
            font-family: monospace;
            font-size: 0.8rem;
            background: rgba(0, 0, 0, 0.3);
            border: 1px solid #3b82f6;
            border-radius: 8px;
            padding: 10px;
            margin-top: 20px;
         }
         .agent-log div {
            margin-bottom: 5px;
            padding-bottom: 5px;
            border-bottom: 1px dashed rgba(255, 255, 255, 0.1);
         }
         .agent-log div:last-child {
             border-bottom: none;
             margin-bottom: 0;
             padding-bottom: 0;
         }
         .log-command { color: #60a5fa; } /* Blue */
         .log-perception { color: #a78bfa; } /* Purple */
         .log-action { color: #34d399; } /* Green */
         .log-read { color: #fcd34d; } /* Yellow */
         .log-error { color: #f87171; } /* Red */
    </style>
</head>
<body class="bg-gray-900 text-gray-100 min-h-screen">
    <div class="gradient-bg fixed top-0 left-0 w-full h-64 -z-10"></div>

    <!-- Header -->
    <header class="py-6 px-6 lg:px-12 flex justify-between items-center">
        <div class="flex items-center space-x-3">
            <div class="bg-blue-500 p-3 rounded-lg">
                <i class="fas fa-robot text-white text-2xl"></i>
            </div>
            <div>
                <h1 class="text-2xl font-bold">Pump.fun Automation Architect</h1>
                <p class="text-blue-300 text-sm">n8n-powered algorithmic trading strategist</p>
            </div>
        </div>
        <div class="flex space-x-4">
            <!-- Deploy Button with Agent Command -->
            <button id="btn-deploy-workflow"
                    class="px-4 py-2 bg-blue-600 hover:bg-blue-700 rounded-lg flex items-center space-x-2 transition"
                    data-agent-command='{"action": "deployWorkflow", "target": "workflow", "workflowId": "PumpFun_MasterEventIngestor_v2"}'>
                <i class="fas fa-play"></i>
                <span>Deploy</span>
            </button>
            <!-- Settings Button with Agent Command -->
            <button id="btn-open-settings"
                    class="px-4 py-2 bg-gray-800 hover:bg-gray-700 rounded-lg flex items-center space-x-2 transition"
                    data-agent-command='{"action": "navigate", "target": "settingsPage"}'>
                <i class="fas fa-cog"></i>
                <span>Settings</span>
            </button>
        </div>
    </header>

    <main class="container mx-auto px-6 lg:px-12 pb-12">
        <!-- Dashboard Overview -->
        <section class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-8">
            <div class="dashboard-card rounded-xl p-6 border border-gray-700" data-dashboard-card="activeWorkflows">
                <div class="flex justify-between items-start">
                    <div>
                        <p class="text-gray-400">Active Workflows</p>
                        <h2 class="text-3xl font-bold mt-1" id="dashboard-active-workflows-count">4</h2>
                    </div>
                    <div class="bg-blue-500/20 p-3 rounded-lg">
                        <i class="fas fa-project-diagram text-blue-400"></i>
                    </div>
                </div>
                <div class="mt-4 pt-4 border-t border-gray-700 flex justify-between">
                    <span class="text-green-400 text-sm" id="dashboard-workflows-today">+2 today</span>
                    <!-- Link to view all workflows with Agent Command -->
                    <a href="#" class="text-blue-400 text-sm hover:underline"
                       data-agent-command='{"action": "navigate", "target": "workflowsListPage"}'>View all</a>
                </div>
            </div>

            <div class="dashboard-card rounded-xl p-6 border border-gray-700" data-dashboard-card="dataStreams">
                <div class="flex justify-between items-start">
                    <div>
                        <p class="text-gray-400">Data Streams</p>
                        <h2 class="text-3xl font-bold mt-1" id="dashboard-data-streams-count">12</h2>
                    </div>
                    <div class="bg-purple-500/20 p-3 rounded-lg">
                        <i class="fas fa-stream text-purple-400"></i>
                    </div>
                </div>
                <div class="mt-4 pt-4 border-t border-gray-700 flex justify-between">
                    <span class="text-green-400 text-sm" id="dashboard-data-streams-rate">3.2K events/min</span>
                    <!-- Link to monitor data streams with Agent Command -->
                    <a href="#" class="text-blue-400 text-sm hover:underline"
                       data-agent-command='{"action": "navigate", "target": "dataStreamsMonitorPage"}'>Monitor</a>
                </div>
            </div>

            <div class="dashboard-card rounded-xl p-6 border border-gray-700" data-dashboard-card="profitability">
                <div class="flex justify-between items-start">
                    <div>
                        <p class="text-gray-400">Profitability</p>
                        <h2 class="text-3xl font-bold mt-1" id="dashboard-profitability">+8.42%</h2>
                    </div>
                    <div class="bg-green-500/20 p-3 rounded-lg">
                        <i class="fas fa-chart-line text-green-400"></i>
                    </div>
                </div>
                <div class="mt-4 pt-4 border-t border-gray-700 flex justify-between">
                    <span class="text-green-400 text-sm">24h performance</span>
                    <!-- Link to analyze profitability with Agent Command -->
                    <a href="#" class="text-blue-400 text-sm hover:underline"
                       data-agent-command='{"action": "navigate", "target": "profitabilityAnalysisPage"}'>Analyze</a>
                </div>
            </div>

            <div class="dashboard-card rounded-xl p-6 border border-gray-700" data-dashboard-card="riskLevel">
                <div class="flex justify-between items-start">
                    <div>
                        <p class="text-gray-400">Risk Level</p>
                        <h2 class="text-3xl font-bold mt-1" id="dashboard-risk-level">Medium</h2>
                    </div>
                    <div class="bg-yellow-500/20 p-3 rounded-lg">
                        <i class="fas fa-shield-alt text-yellow-400"></i>
                    </div>
                </div>
                <div class="mt-4 pt-4 border-t border-gray-700 flex justify-between">
                    <span class="text-yellow-400 text-sm" id="dashboard-risk-alerts">3 active alerts</span>
                    <!-- Link to review risk alerts with Agent Command -->
                    <a href="#" class="text-blue-400 text-sm hover:underline"
                       data-agent-command='{"action": "navigate", "target": "riskAlertsPage"}'>Review</a>
                </div>
            </div>
        </section>

        <!-- Workflow Builder Section -->
        <section class="mb-12">
            <div class="flex justify-between items-center mb-6">
                <h2 class="text-xl font-bold">Phase 1: Real-Time pump.fun Intelligence Network</h2>
                <div class="flex space-x-3">
                    <!-- Add Node Button with Agent Command -->
                    <button id="btn-add-node"
                            class="px-4 py-2 bg-blue-600 hover:bg-blue-700 rounded-lg flex items-center space-x-2 transition"
                            data-agent-command='{"action": "openNodePalette", "target": "workflowCanvas"}'>
                        <i class="fas fa-plus"></i>
                        <span>Add Node</span>
                    </button>
                    <!-- Save Workflow Button with Agent Command -->
                    <button id="btn-save-workflow"
                            class="px-4 py-2 bg-gray-800 hover:bg-gray-700 rounded-lg flex items-center space-x-2 transition"
                            data-agent-command='{"action": "saveWorkflow", "target": "workflow", "workflowId": "PumpFun_MasterEventIngestor_v2"}'>
                        <i class="fas fa-save"></i>
                        <span>Save Workflow</span>
                    </button>
                </div>
            </div>

            <div id="workflow-canvas-container"
                 class="bg-gray-800 rounded-xl p-6 border border-gray-700 relative overflow-hidden"
                 data-workflow-id="PumpFun_MasterEventIngestor_v2"
                 data-selected-node-id=""> <!-- Attribute to track selected node for the agent -->
                <!-- Workflow Canvas -->
                <div id="workflow-canvas" class="relative min-h-[400px]">
                    <!-- START Node with ID, Data Attributes, and Agent Command -->
                    <div id="node-start"
                         class="node-container absolute top-6 left-1/2 transform -translate-x-1/2 z-10"
                         data-node-id="node-start"
                         data-node-type="Start"
                         data-agent-command='{"action": "selectNode", "target": "#node-start"}'>
                        <div class="bg-green-600 rounded-lg p-4 w-40 text-center shadow-lg">
                            <div class="flex justify-center mb-2">
                                <div class="bg-green-700 p-2 rounded-full">
                                    <i class="fas fa-play text-white"></i>
                                </div>
                            </div>
                            <h3 class="font-bold">START</h3>
                            <p class="text-xs text-green-200 mt-1">Workflow trigger</p>
                        </div>
                    </div>

                    <!-- Connection Lines (Static for this example) -->
                    <div class="node-connection-line" style="width: 200px; top: 120px; left: calc(50% - 100px);"></div>
                    <!-- Lines from START to the two WebSockets -->
                    <div class="node-connection-line" style="width: 160px; top: 140px; left: calc(50% - 80px); transform-origin: right center; transform: rotate(30deg);"></div>
                    <div class="node-connection-line" style="width: 160px; top: 140px; left: calc(50% - 80px); transform-origin: left center; transform: rotate(-30deg);"></div>


                    <!-- WebSocket Nodes with IDs, Data Attributes, and Agent Commands -->
                    <div id="node-new-tokens"
                         class="node-container absolute top-48 left-1/4 transform -translate-x-1/2 z-10"
                         data-node-id="node-new-tokens"
                         data-node-type="WebSocket"
                         data-agent-command='{"action": "selectNode", "target": "#node-new-tokens"}'>
                        <div class="bg-blue-600 rounded-lg p-4 w-48 text-center shadow-lg pulse-animation">
                            <div class="flex justify-center mb-2">
                                <div class="bg-blue-700 p-2 rounded-full">
                                    <i class="fas fa-plug text-white"></i>
                                </div>
                            </div>
                            <h3 class="font-bold">New Tokens Stream</h3>
                            <p class="text-xs text-blue-200 mt-1">bloXroute WebSocket</p>
                            <div class="mt-3 flex justify-center space-x-2">
                                <span class="bg-blue-700 text-xs px-2 py-1 rounded" data-status-field="connection">Active</span>
                                <span class="bg-gray-700 text-xs px-2 py-1 rounded" data-status-field="rate">3.2K/min</span>
                            </div>
                        </div>
                    </div>

                    <div id="node-swaps"
                         class="node-container absolute top-48 left-3/4 transform -translate-x-1/2 z-10"
                         data-node-id="node-swaps"
                         data-node-type="WebSocket"
                         data-agent-command='{"action": "selectNode", "target": "#node-swaps"}'>
                        <div class="bg-purple-600 rounded-lg p-4 w-48 text-center shadow-lg pulse-animation">
                            <div class="flex justify-center mb-2">
                                <div class="bg-purple-700 p-2 rounded-full">
                                    <i class="fas fa-exchange-alt text-white"></i>
                                </div>
                            </div>
                            <h3 class="font-bold">Swaps Stream</h3>
                            <p class="text-xs text-purple-200 mt-1">bloXroute WebSocket</p>
                            <div class="mt-3 flex justify-center space-x-2">
                                <span class="bg-purple-700 text-xs px-2 py-1 rounded" data-status-field="connection">Active</span>
                                <span class="bg-gray-700 text-xs px-2 py-1 rounded" data-status-field="rate">5.7K/min</span>
                            </div>
                        </div>
                    </div>

                    <!-- Connection Lines from WebSockets to Standardize -->
                    <div class="node-connection-line" style="width: 160px; top: 340px; left: calc(25% - 80px); transform-origin: right center; transform: rotate(-30deg);"></div>
                    <div class="node-connection-line" style="width: 160px; top: 340px; left: calc(75% - 80px); transform-origin: left center; transform: rotate(30deg);"></div>


                    <!-- Standardization Node with ID, Data Attributes, and Agent Command -->
                    <div id="node-standardize"
                         class="node-container absolute top-72 left-1/2 transform -translate-x-1/2 z-10"
                         data-node-id="node-standardize"
                         data-node-type="Function"
                         data-agent-command='{"action": "selectNode", "target": "#node-standardize"}'>
                        <div class="bg-indigo-600 rounded-lg p-4 w-52 text-center shadow-lg">
                            <div class="flex justify-center mb-2">
                                <div class="bg-indigo-700 p-2 rounded-full">
                                    <i class="fas fa-code text-white"></i>
                                </div>
                            </div>
                            <h3 class="font-bold">Standardize & Route</h3>
                            <p class="text-xs text-indigo-200 mt-1">Function Node</p>
                            <div class="mt-3">
                                <span class="bg-indigo-700 text-xs px-2 py-1 rounded" data-status-field="processing">Processing</span>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Node Configuration Panel -->
                <div id="node-config-panel" class="mt-16 bg-gray-800 rounded-lg border border-gray-700 p-6" data-panel-type="nodeConfig">
                    <div class="flex justify-between items-center mb-4">
                        <h3 class="font-bold text-lg">Node Configuration</h3>
                        <div class="flex space-x-2">
                            <!-- Copy Node Button with Agent Command -->
                            <button id="btn-copy-node"
                                    class="bg-gray-700 hover:bg-gray-600 p-2 rounded-lg"
                                    data-agent-command='{"action": "copyNode", "target": "selectedNode"}'>
                                <i class="fas fa-copy"></i>
                            </button>
                            <!-- Delete Node Button with Agent Command -->
                            <button id="btn-delete-node"
                                    class="bg-gray-700 hover:bg-gray-600 p-2 rounded-lg"
                                    data-agent-command='{"action": "deleteNode", "target": "selectedNode"}'>
                                <i class="fas fa-trash-alt"></i>
                            </button>
                        </div>
                    </div>

                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                        <div>
                            <label for="config-node-name" class="block text-sm font-medium text-gray-400 mb-1">Node Name</label>
                            <!-- Node Name Input with ID and Data Attribute -->
                            <input type="text" id="config-node-name" value="" class="w-full bg-gray-700 border border-gray-600 rounded-lg px-3 py-2" data-config-field="name">
                        </div>
                        <div>
                            <label for="config-node-type" class="block text-sm font-medium text-gray-400 mb-1">Node Type</label>
                            <!-- Node Type Select with ID and Data Attribute -->
                            <select id="config-node-type" class="w-full bg-gray-700 border border-gray-600 rounded-lg px-3 py-2" data-config-field="type">
                                <option value="Start">Start</option>
                                <option value="Function">Function</option>
                                <option value="HTTP Request">HTTP Request</option>
                                <option value="Webhook">Webhook</option>
                                <option value="Condition">Condition</option>
                                <option value="WebSocket">WebSocket</option>
                                <option value="Database">Database</option>
                                <option value="API">API</option>
                            </select>
                        </div>
                    </div>

                    <div class="mt-4">
                        <label for="config-node-code" class="block text-sm font-medium text-gray-400 mb-1">Function Code</label>
                        <!-- Function Code Display/Editor Area with ID and Data Attribute -->
                        <!-- Using a contenteditable div to simulate an editor the agent could theoretically interact with -->
                        <div id="config-node-code"
                             class="bg-gray-900 rounded-lg p-4 font-mono text-sm overflow-x-auto text-gray-300"
                             contenteditable="true"
                             spellcheck="false"
                             data-config-field="code"
                             style="min-height: 150px; white-space: pre-wrap;"></div>
                    </div>

                    <div class="mt-6 flex justify-end space-x-3">
                        <!-- Cancel Button with Agent Command -->
                        <button id="btn-cancel-config"
                                class="px-4 py-2 bg-gray-700 hover:bg-gray-600 rounded-lg"
                                data-agent-command='{"action": "cancelConfig", "target": "#node-config-panel"}'>
                            Cancel
                        </button>
                        <!-- Save Changes Button with Agent Command -->
                        <button id="btn-save-config"
                                class="px-4 py-2 bg-blue-600 hover:bg-blue-700 rounded-lg"
                                data-agent-command='{"action": "saveConfig", "target": "selectedNode", "readFields": ["#config-node-name", "#config-node-type", "#config-node-code"]}'>
                            Save Changes
                        </button>
                    </div>
                </div>
            </div>
        </section>

        <!-- Recent Activity -->
        <section>
            <div class="flex justify-between items-center mb-6">
                <h2 class="text-xl font-bold">Recent Activity</h2>
                <!-- Link to view all activity with Agent Command -->
                <button class="text-blue-400 hover:underline"
                        data-agent-command='{"action": "navigate", "target": "activityLogPage"}'>View all activity</button>
            </div>

            <div class="bg-gray-800 rounded-xl border border-gray-700 overflow-hidden" data-section="activityLog">
                <div class="overflow-x-auto">
                    <table class="w-full">
                        <thead class="bg-gray-700">
                            <tr>
                                <th class="px-6 py-3 text-left text-xs font-medium text-gray-300 uppercase tracking-wider">Timestamp</th>
                                <th class="px-6 py-3 text-left text-xs font-medium text-gray-300 uppercase tracking-wider">Event</th>
                                <th class="px-6 py-3 text-left text-xs font-medium text-gray-300 uppercase tracking-wider">Workflow</th>
                                <th class="px-6 py-3 text-left text-xs font-medium text-gray-300 uppercase tracking-wider">Status</th>
                            </tr>
                        </thead>
                        <tbody id="activity-log-body" class="divide-y divide-gray-700">
                            <!-- Activity rows will be added/updated by JS simulation -->
                        </tbody>
                    </table>
                </div>
            </div>
        </section>

        <!-- Agent Simulation Log -->
        <section class="mt-12">
             <h2 class="text-xl font-bold mb-4">Agent Simulation Log</h2>
             <div id="agent-log" class="agent-log">
                 <div><span class="log-perception">Perception:</span> UI loaded. Ready for commands or observations.</div>
             </div>
        </section>
    </main>

    <footer class="bg-gray-800 border-t border-gray-700 py-6 mt-12">
        <div class="container mx-auto px-6 lg:px-12">
            <div class="flex flex-col md:flex-row justify-between items-center">
                <div class="flex items-center space-x-3 mb-4 md:mb-0">
                    <div class="bg-blue-500 p-2 rounded-lg">
                        <i class="fas fa-robot text-white"></i>
                    </div>
                    <span class="font-medium">Pump.fun Automation Architect</span>
                </div>
                <div class="flex space-x-6">
                    <a href="#" class="text-gray-400 hover:text-white">Documentation</a>
                    <a href="#" class="text-gray-400 hover:text-white">API Reference</a>
                    <a href="#" class="text-gray-400 hover:text-white">Support</a>
                </div>
            </div>
            <div class="mt-6 text-center md:text-left text-sm text-gray-500">
                © 2023 Pump.fun Automation Architect. All rights reserved.
            </div>
        </div>
    </footer>

    <script>
        // --- Agent Simulation Logic ---
        const agentLog = document.getElementById('agent-log');
        const workflowCanvasContainer = document.getElementById('workflow-canvas-container');
        const nodeConfigPanel = document.getElementById('node-config-panel');
        const nodeNameInput = document.getElementById('config-node-name');
        const nodeTypeSelect = document.getElementById('config-node-type');
        const nodeCodeDiv = document.getElementById('config-node-code'); // Use div for contenteditable

        // Simulate node data (in a real app, this would come from backend/state)
        const nodeData = {
            "node-start": { name: "START", type: "Start", code: "// Workflow Start" },
            "node-new-tokens": { name: "New Tokens Stream", type: "WebSocket", code: "// WebSocket config for new tokens\n// Endpoint: wss://api.bloxroute.com/v1/ws\n// Subscribe: newTokens" },
            "node-swaps": { name: "Swaps Stream", type: "WebSocket", code: "// WebSocket config for swaps\n// Endpoint: wss://api.bloxroute.com/v1/ws\n// Subscribe: swaps" },
            "node-standardize": { name: "Standardize & Route", type: "Function", code: `// Standardize and route incoming stream data
const streamData = items[0].json;
const standardizedData = {
  timestamp: Date.now(),
  eventType: streamData.method, // Assuming method indicates event type
  data: streamData.params // Assuming params contains the payload
};

// Example routing logic (placeholder)
if (standardizedData.eventType === 'newTokens') {
    // Route to New Token Handler workflow/node
    console.log('Routing new token data:', standardizedData);
    // return [{ json: standardizedData, next: 'newTokenHandlerNodeId' }]; // Example n8n routing
} else if (standardizedData.eventType === 'swaps') {
    // Route to Swap Handler workflow/node
    console.log('Routing swap data:', standardizedData);
    // return [{ json: standardizedData, next: 'swapHandlerNodeId' }]; // Example n8n routing
} else {
    // Handle unknown event types
    console.warn('Unknown event type received:', standardizedData.eventType);
    // return [{ json: standardizedData, next: 'errorHandlerNodeId' }];
}

// For this simple example, just return the standardized data
return [{ json: standardizedData }];
` } // Note: Using backticks for multi-line string
        };

        let selectedNodeId = null; // Track selected node ID in JS state

        function logAgentMessage(message, type = 'action') {
            const logEntry = document.createElement('div');
            logEntry.classList.add(`log-${type}`);
            logEntry.innerHTML = `<span class="font-bold capitalize">${type}:</span> ${message}`;
            agentLog.prepend(logEntry); // Add to top
            // Keep log size reasonable
            while (agentLog.children.length > 20) {
                agentLog.removeChild(agentLog.lastChild);
            }
        }

        function updateConfigPanel(nodeId) {
            // UI Selection Logic: Remove selected class from all nodes, add to selected
            document.querySelectorAll('.node-container').forEach(n => n.classList.remove('selected'));
            if (nodeId) {
                const nodeElement = document.getElementById(nodeId);
                if (nodeElement) {
                     nodeElement.classList.add('selected');
                }
            }

            const data = nodeData[nodeId];
            if (data) {
                nodeNameInput.value = data.name;
                nodeTypeSelect.value = data.type;
                nodeCodeDiv.textContent = data.code; // Use textContent for contenteditable div
                // Show the panel (assuming it might be hidden initially in a real app)
                nodeConfigPanel.style.display = 'block';
            } else {
                 // Clear and hide the panel if no node is selected or data is missing
                nodeNameInput.value = '';
                nodeTypeSelect.value = '';
                nodeCodeDiv.textContent = '';
                nodeConfigPanel.style.display = 'none'; // Hide panel
            }
        }

        // Add event listeners to elements with data-agent-command attributes
        document.querySelectorAll('[data-agent-command]').forEach(element => {
            element.addEventListener('click', function(event) {
                event.preventDefault(); // Prevent default link/button behavior
                const commandString = this.getAttribute('data-agent-command');
                try {
                    const command = JSON.parse(commandString);
                    logAgentMessage(`Command triggered: ${JSON.stringify(command)}`, 'command');

                    // --- Agent Action Simulation & Perception ---
                    // This simulates the agent receiving the command and performing actions.
                    // In a real agent, this would involve interacting with the DOM,
                    // reading attributes, and potentially calling backend APIs.

                    const action = command.action;
                    const target = command.target;
                    const params = command.params || {};

                    switch (action) {
                        case 'deployWorkflow':
                            logAgentMessage(`Attempting to deploy workflow ${command.workflowId}...`, 'action');
                            // Simulate API call
                            setTimeout(() => {
                                logAgentMessage(`Workflow ${command.workflowId} deployed successfully.`, 'action');
                                // Simulate UI update (e.g., status indicator)
                            }, 1500);
                            break;
                        case 'navigate':
                            logAgentMessage(`Navigating to ${target}...`, 'action');
                            // Simulate route change or showing a different section
                            // In this static HTML, we just log it.
                            break;
                        case 'openNodePalette':
                            logAgentMessage(`Opening node palette for ${target}...`, 'action');
                            // Simulate showing a modal or sidebar with node options
                            break;
                        case 'saveWorkflow':
                             logAgentMessage(`Attempting to save workflow ${command.workflowId}...`, 'action');
                             // Simulate gathering workflow state and sending to backend
                            setTimeout(() => {
                                logAgentMessage(`Workflow ${command.workflowId} saved successfully.`, 'action');
                            }, 1000);
                            break;
                        case 'selectNode':
                            // Agent perceives the click on a node element (target is the node's selector)
                            const clickedNodeElement = document.querySelector(target);
                            if (clickedNodeElement) {
                                const nodeId = clickedNodeElement.getAttribute('data-node-id');
                                logAgentMessage(`Perception: User/Agent clicked node element ${target}. Node ID: ${nodeId}`, 'perception');

                                // Simulate agent updating its internal state and the UI
                                selectedNodeId = nodeId;
                                workflowCanvasContainer.setAttribute('data-selected-node-id', nodeId); // Update canvas attribute

                                // Simulate agent updating the config panel based on the selected node
                                logAgentMessage(`Action: Updating config panel for node ${nodeId}.`, 'action');
                                updateConfigPanel(nodeId); // This also handles adding the 'selected' class via JS

                                // Simulate agent perceiving the updated config panel state
                                setTimeout(() => {
                                     logAgentMessage(`Perception: Config panel updated for node ${nodeId}. Name: "${nodeNameInput.value}", Type: "${nodeTypeSelect.value}".`, 'perception');
                                }, 100); // Small delay to simulate perception after UI update
                            } else {
                                logAgentMessage(`Error: Could not find node element ${target} to select.`, 'error');
                            }
                            break;
                        case 'copyNode':
                            // Agent needs to know which node is selected
                            const nodeToCopyId = workflowCanvasContainer.getAttribute('data-selected-node-id');
                            if (nodeToCopyId && nodeData[nodeToCopyId]) {
                                logAgentMessage(`Attempting to copy node ${nodeToCopyId}...`, 'action');
                                // Simulate creating a new node object
                                const newNodeId = `node-copy-${Date.now()}`; // Simple unique ID
                                nodeData[newNodeId] = { ...nodeData[nodeToCopyId], name: `Copy of ${nodeData[nodeToCopyId].name}` };
                                logAgentMessage(`Simulated copy: Created new node data with ID ${newNodeId}.`, 'action');
                                // In a real app, agent would add this node to the canvas DOM
                                // For simulation, we just log the data update.
                            } else {
                                logAgentMessage(`Error: No node selected or data not found for copy action.`, 'error');
                            }
                            break;
                        case 'deleteNode':
                             const nodeToDeleteId = workflowCanvasContainer.getAttribute('data-selected-node-id');
                             if (nodeToDeleteId && nodeToDeleteId !== 'node-start') { // Prevent deleting START node
                                logAgentMessage(`Attempting to delete node ${nodeToDeleteId}...`, 'action');
                                // Simulate removing node data
                                delete nodeData[nodeToDeleteId];
                                logAgentMessage(`Simulated delete: Removed node data for ${nodeToDeleteId}.`, 'action');
                                // Simulate removing node from UI (if it existed)
                                const nodeElement = document.getElementById(nodeToDeleteId);
                                if(nodeElement) {
                                    nodeElement.remove();
                                }
                                // Simulate clearing config panel
                                selectedNodeId = null;
                                workflowCanvasContainer.setAttribute('data-selected-node-id', '');
                                updateConfigPanel(null); // Pass null to clear and hide panel
                                logAgentMessage(`Action: Cleared config panel.`, 'action');

                             } else if (nodeToDeleteId === 'node-start') {
                                logAgentMessage(`Error: Cannot delete the START node.`, 'error');
                             }
                             else {
                                logAgentMessage(`Error: No node selected for delete action.`, 'error');
                            }
                            break;
                        case 'cancelConfig':
                            logAgentMessage(`Cancelling configuration changes for ${target}...`, 'action');
                            // Simulate reverting panel state or hiding it
                            updateConfigPanel(selectedNodeId); // Revert to currently loaded data
                            logAgentMessage(`Action: Reverted config panel to current node data.`, 'action');
                            break;
                        case 'saveConfig':
                            const nodeToSaveId = workflowCanvasContainer.getAttribute('data-selected-node-id');
                            if (nodeToSaveId && nodeData[nodeToSaveId]) {
                                logAgentMessage(`Attempting to save config for node ${nodeToSaveId}...`, 'action');
                                const readFields = params.readFields || [];
                                const configUpdates = {};

                                // Simulate agent reading values from the UI elements *before* saving
                                readFields.forEach(fieldSelector => {
                                    const fieldElement = document.querySelector(fieldSelector);
                                    if (fieldElement) {
                                        const fieldName = fieldElement.getAttribute('data-config-field');
                                        let value;
                                        if (fieldElement.tagName === 'INPUT' || fieldElement.tagName === 'SELECT') {
                                            value = fieldElement.value;
                                        } else if (fieldElement.getAttribute('contenteditable') === 'true') {
                                            value = fieldElement.textContent; // Read contenteditable div
                                        }
                                        if (fieldName) {
                                            configUpdates[fieldName] = value;
                                            logAgentMessage(`Perception: Read field "${fieldName}" (${fieldSelector}) with value "${value}".`, 'read');
                                        } else {
                                             logAgentMessage(`Perception: Read element ${fieldSelector} with value "${value}", but no data-config-field found.`, 'read');
                                        }
                                    } else {
                                        logAgentMessage(`Error: Could not find config field element ${fieldSelector} to read for save.`, 'error');
                                    }
                                });

                                // Simulate applying updates to node data
                                logAgentMessage(`Action: Applying updates to node data for ${nodeToSaveId}: ${JSON.stringify(configUpdates)}`, 'action');
                                nodeData[nodeToSaveId] = { ...nodeData[nodeToSaveId], ...configUpdates };

                                // Simulate backend save API call
                                setTimeout(() => {
                                    logAgentMessage(`Config for node ${nodeToSaveId} saved successfully.`, 'action');
                                    // Simulate UI update if necessary (e.g., node name on canvas)
                                    const nodeElement = document.getElementById(nodeToSaveId);
                                    if (nodeElement) {
                                         const nameElement = nodeElement.querySelector('h3');
                                         if (nameElement && configUpdates.name !== undefined) {
                                            nameElement.textContent = configUpdates.name;
                                            logAgentMessage(`Action: Updated node name on canvas for ${nodeToSaveId}.`, 'action');
                                         }
                                    }
                                }, 1000);

                            } else {
                                logAgentMessage(`Error: No node selected or data not found for save action.`, 'error');
                            }
                            break;
                        default:
                            logAgentMessage(`Warning: Unknown agent action "${action}".`, 'error');
                    }

                } catch (e) {
                    console.error("Failed to parse or execute agent command:", commandString, e);
                    logAgentMessage(`Error: Failed to parse or execute command: ${commandString}. Details: ${e.message}`, 'error');
                }
            });
        });

        // --- UI Interaction Logic (Manual User Clicks) ---
        // This part handles the visual selection and panel update when a user clicks a node,
        // independent of the agent simulation triggered by data-agent-command.
        // In a real agent scenario, the agent might *observe* these manual clicks
        // and update its internal state accordingly, or trigger the 'selectNode' command itself.

        document.querySelectorAll('.node-container').forEach(node => {
            node.addEventListener('click', function(event) {
                // This click handler simulates both the UI update AND the agent command
                // In a real scenario, the agent might *only* rely on triggering the data-agent-command,
                // or it might observe the manual click and *then* trigger the command internally.
                // For this simulation, the click directly triggers the command handler logic.
            });
        });


        // --- Activity Log Simulation ---
        const activityBody = document.getElementById('activity-log-body');
        const initialActivity = [
             { timestamp: '2023-07-15 14:32:45', event: 'New token detected', workflow: 'PumpFun_MasterEventIngestor_v2', status: 'Processed', statusClass: 'bg-green-900 text-green-300' },
             { timestamp: '2023-07-15 14:31:22', event: 'Large swap detected', workflow: 'PumpFun_MasterEventIngestor_v2', status: 'Analyzing', statusClass: 'bg-yellow-900 text-yellow-300' },
             { timestamp: '2023-07-15 14:30:18', event: 'WebSocket reconnected', workflow: 'Stream_PumpFunSwaps_bloXroute', status: 'Success', statusClass: 'bg-green-900 text-green-300' },
             { timestamp: '2023-07-15 14:28:56', event: 'Risk threshold exceeded', workflow: 'RiskManagement_v1', status: 'Alert', statusClass: 'bg-red-900 text-red-300' },
             { timestamp: '2023-07-15 14:27:10', event: 'Node "Standardize" processed item', workflow: 'PumpFun_MasterEventIngestor_v2', status: 'Processed', statusClass: 'bg-green-900 text-green-300' },
        ];

        function addActivityRow(entry) {
             const newRow = `
                 <tr>
                     <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-300">${entry.timestamp}</td>
                     <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-white">${entry.event}</td>
                     <td class="px-6 py-4 whitespace-nowrap text-sm text-blue-400">${entry.workflow}</td>
                     <td class="px-6 py-4 whitespace-nowrap">
                         <span class="px-2 inline-flex text-xs leading-5 font-semibold rounded-full ${entry.statusClass}">${entry.status}</span>
                     </td>
                 </tr>
             `;
             activityBody.insertAdjacentHTML('afterbegin', newRow);
             // Keep table size reasonable (e.g., max 10 rows)
             while (activityBody.rows.length > 10) {
                 activityBody.deleteRow(activityBody.rows.length - 1);
             }
        }

        // Populate initial activity
        initialActivity.forEach(entry => addActivityRow(entry));


        setInterval(() => {
            // Simulate status changes
            const rows = activityBody.querySelectorAll('tr');
            rows.forEach(row => {
                const statusCellSpan = row.querySelector('td:last-child span');
                const currentStatus = statusCellSpan.textContent;
                let newStatus = currentStatus;
                let newClass = statusCellSpan.className;

                if (Math.random() > 0.6) { // Randomly decide to change status
                    if (currentStatus === 'Processed' && Math.random() > 0.5) {
                         newStatus = 'Analyzing'; newClass = 'px-2 inline-flex text-xs leading-5 font-semibold rounded-full bg-yellow-900 text-yellow-300';
                    } else if (currentStatus === 'Analyzing' && Math.random() > 0.5) {
                         newStatus = 'Processed'; newClass = 'px-2 inline-flex text-xs leading-5 font-semibold rounded-full bg-green-900 text-green-300';
                    } else if (currentStatus === 'Alert' && Math.random() > 0.7) {
                         newStatus = 'Resolved'; newClass = 'px-2 inline-flex text-xs leading-5 font-semibold rounded-full bg-green-900 text-green-300';
                    } else if (currentStatus === 'Resolved' && Math.random() > 0.9) {
                         newStatus = 'Processed'; newClass = 'px-2 inline-flex text-xs leading-5 font-semibold rounded-full bg-green-900 text-green-300';
                    } else if (currentStatus === 'Success' && Math.random() > 0.95) {
                         newStatus = 'Failed'; newClass = 'px-2 inline-flex text-xs leading-5 font-semibold rounded-full bg-red-900 text-red-300';
                    }
                }

                if (newStatus !== currentStatus) {
                    // logAgentMessage(`Perception: Activity status changed from "${currentStatus}" to "${newStatus}" for event "${row.querySelector('td:nth-child(2)').textContent}".`, 'perception');
                    statusCellSpan.textContent = newStatus;
                    statusCellSpan.className = newClass; // Update classes
                }
            });

            // Add a new random activity entry occasionally
            if (Math.random() > 0.7) { // More frequent new entries
                const now = new Date();
                const timestamp = `${now.getFullYear()}-${String(now.getMonth() + 1).padStart(2, '0')}-${String(now.getDate()).padStart(2, '0')} ${String(now.getHours()).padStart(2, '0')}:${String(now.getMinutes()).padStart(2, '0')}:${String(now.getSeconds()).padStart(2, '0')}`;
                const events = [
                    { event: 'Minor price swing detected', workflow: 'PumpFun_MasterEventIngestor_v2', status: 'Processed', statusClass: 'bg-green-900 text-green-300' },
                    { event: 'Transaction confirmed', workflow: 'PumpFun_TradingBot_v1', status: 'Success', statusClass: 'bg-green-900 text-green-300' },
                    { event: 'API rate limit warning', workflow: 'RiskManagement_v1', status: 'Warning', statusClass: 'bg-yellow-900 text-yellow-300' },
                    { event: 'New token analysis started', workflow: 'PumpFun_NewTokenAnalyzer_v1', status: 'Processing', statusClass: 'bg-yellow-900 text-yellow-300' },
                    { event: 'Data stream latency high', workflow: 'Stream_PumpFunSwaps_bloXroute', status: 'Alert', statusClass: 'bg-red-900 text-red-300' },
                     { event: 'Config change deployed', workflow: 'System', status: 'Success', statusClass: 'bg-green-900 text-green-300' },
                ];
                const randomEvent = events[Math.floor(Math.random() * events.length)];
                addActivityRow({ ...randomEvent, timestamp: timestamp });

                 // Simulate agent perceiving the new entry
                 // logAgentMessage(`Perception: New activity entry appeared: "${randomEvent.event}".`, 'perception');
            }

        }, 2000); // Update every 2 seconds


        // Initial state setup: Clear config panel until a node is selected
        updateConfigPanel(null);

    });
    </script>
</body>
</html>
<span class="log-perception">Perception:</span> Agent Operational Protocol (Comprehensive Adhesive Mode) received and fully processed. I understand my mission, environment, and the strict adherence required to `data-*` attributes for perception and action triggering.
<span class="log-perception">Perception:</span> Scanning the initial UI state based on the provided HTML.
<span class="log-perception">Perception:</span> Identified #agent-log element.
<span class="log-perception">Perception:</span> Read initial log message in #agent-log: "UI loaded. Ready for commands or observations."
<span class="log-perception">Perception:</span> Identified #workflow-canvas-container with data-selected-node-id="". Confirmed no node is initially selected.
<span class="log-perception">Perception:</span> Identified #node-config-panel element. Confirmed it is initially hidden as per simulation script logic when no node is selected.
I am now fully operational and awaiting your first specific instruction, which I will process according to the detailed protocol above.




1. User Interface Layout

Header

Title: "AI Assistant Dashboard"
User Profile: Display user name and profile picture
Notifications: Bell icon for alerts and updates
Settings: Gear icon for configuration options
Sidebar Navigation

Home: Overview of key functionalities
Tools: Access to various tools and utilities
Reports: Generate and view reports
Settings: Customize dashboard preferences
Help: Access to documentation and support
Main Content Area

Widgets/Sections:
Quick Actions: Buttons for frequently used commands (e.g., "Create New Task", "Generate Report")
Data Visualization: Charts and graphs displaying key metrics (e.g., performance, usage statistics)
Recent Activity Log: List of recent actions taken by the user or system
Task Manager: Overview of tasks with statuses (e.g., pending, completed)
Calendar Integration: Display upcoming events and deadlines
2. Tools and Functionalities

Task Management Tool

Create, edit, and delete tasks
Set deadlines and reminders
Categorize tasks by priority or project
Data Analysis Tool

Import data from various sources (CSV, Excel, APIs)
Perform data cleaning and transformation
Generate visualizations (bar charts, line graphs, pie charts)
Report Generation Tool

Create customizable reports based on user-defined parameters
Export reports in multiple formats (PDF, Excel, Word)
Schedule automated report generation
Communication Tool

Integrated chat feature for real-time communication
Email notifications for important updates
Integration with messaging platforms (Slack, Microsoft Teams)
3. User Customization Options

Theme Selection

Light and dark mode options
Customizable color schemes
Widget Arrangement

Drag-and-drop functionality to rearrange dashboard widgets
Option to add or remove widgets based on user preference
Notification Settings

Customize alert preferences (e.g., email, in-app notifications)
Set quiet hours for notifications
4. Help and Support Section

Documentation

Comprehensive user guide covering all functionalities
FAQs for common issues and troubleshooting
Support Ticket System

Submit support requests directly from the dashboard
Track the status of submitted tickets
Feedback Mechanism

Option for users to provide feedback on the dashboard and tools
Regular surveys to gather user insights for improvements
5. Security Features

User Authentication

Secure login with multi-factor authentication
Role-based access control for sensitive functionalities
Data Encryption

Ensure all data is encrypted in transit and at rest
Regular security audits to identify vulnerabilities