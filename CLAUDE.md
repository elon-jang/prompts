# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This repository contains AI prompt templates for managing Tencent Cloud CVM (Cloud Virtual Machine) instances using the `tccli` command-line tool. The prompts are designed to be used with AI assistants that have access to Terminal MCP (Model Context Protocol) or as Claude Code slash commands.

## Repository Structure

```
/
├── tccli/
│   └── prompts/          # Tencent Cloud VM management prompts
│       ├── README.md     # Korean documentation for prompt usage
│       ├── vm-start.md   # VM instance start prompt
│       ├── vm-stop.md    # VM instance stop prompt
│       ├── vm-status.md  # VM instance status query prompt
│       └── vm-manage.md  # Unified VM start/stop prompt
└── .gitignore
```

## Prompt Architecture

All prompts in `tccli/prompts/` follow a consistent structure:

1. **Role**: Defines the AI as a Tencent Cloud infrastructure management expert
2. **Task**: Specifies the operation (start, stop, status query)
3. **Context**: Provides tool name, operation type, and default instance ID
4. **Instructions**: Step-by-step execution flow with validation points
5. **Commands**: Actual tccli commands with examples and expected outputs
6. **Status Reference**: Tencent Cloud instance state codes

### Key Design Patterns

- **Instance ID Handling**: All prompts use a default instance ID (`ins-1rqliwb5`) but allow user override
- **Terminal Session Isolation**: Each tccli command must run in a new `terminal_start_process` to prevent output mixing
- **API Response Validation**: Prompts verify API responses contain expected fields (`RequestId`, `TotalCount`, `InstanceStatusSet`)
- **Timeout Configuration**: All API calls require `timeout_ms: 30000` or higher to ensure complete responses
- **State Verification**: After state-changing operations, prompts wait 10-15 seconds then verify with `DescribeInstancesStatus`

## Critical Implementation Requirements

### When Working with tccli Prompts

1. **New Terminal Process Required**: Every tccli command must use `terminal_start_process` with a fresh process
   - Prevents command output from being mixed with previous commands
   - Each API call should be isolated in its own terminal session

2. **Timeout Settings**: API calls require minimum 30 seconds timeout
   ```bash
   # Correct usage in terminal_start_process
   timeout_ms: 30000  # or higher
   ```

3. **Response Validation Logic**:
   - **Start/Stop Operations**: Success response contains only `RequestId`
   - **Status Query**: Success response contains `TotalCount`, `InstanceStatusSet`, and `RequestId`
   - If unexpected fields appear, output may be mixed from different commands

4. **Wait Times**: After state-changing operations (start/stop), always wait 10-15 seconds before status verification
   ```bash
   sleep 12
   ```

## Instance State Codes

- `RUNNING`: Instance is running (expected after successful start)
- `STOPPED`: Instance is stopped (expected after successful stop)
- `STARTING`: Start operation in progress
- `STOPPING`: Stop operation in progress
- `PENDING`: Instance being created
- `REBOOTING`: Instance restarting
- `SHUTDOWN`: Instance shut down
- `TERMINATING`: Instance being deleted
- `LAUNCH_FAILED`: Instance creation failed

## Common Commands

### VM Instance Management

```bash
# Check instance status
tccli cvm DescribeInstancesStatus --InstanceIds '["ins-1rqliwb5"]'

# Start instance
tccli cvm StartInstances --InstanceIds '["ins-1rqliwb5"]'

# Stop instance
tccli cvm StopInstances --InstanceIds '["ins-1rqliwb5"]'

# Reboot instance
tccli cvm RebootInstances --InstanceIds '["ins-1rqliwb5"]'

# Query full instance details
tccli cvm DescribeInstances --InstanceIds '["ins-1rqliwb5"]'
```

### Multiple Instances

All commands support multiple instances using JSON array format:
```bash
tccli cvm StartInstances --InstanceIds '["ins-1rqliwb5","ins-abc123xyz"]'
```

## Using Prompts as Slash Commands

To use these prompts as Claude Code slash commands:

```bash
# Copy prompts to Claude Code commands directory
cp tccli/prompts/vm-start.md .claude/commands/vm-start.md
cp tccli/prompts/vm-stop.md .claude/commands/vm-stop.md
cp tccli/prompts/vm-status.md .claude/commands/vm-status.md
cp tccli/prompts/vm-manage.md .claude/commands/vm-manage.md
```

Then use as:
```
/vm-start
/vm-stop
/vm-status
/vm-manage
```

## Prerequisites

- **tccli installation**: `pip install tccli`
- **Tencent Cloud authentication**: Configured credentials for API access
- **Instance ID**: Default is `ins-1rqliwb5`, replace in prompts for different instances

## Customization

To manage different instances, update the instance ID in the relevant prompt file:
- Find: `ins-1rqliwb5`
- Replace with: your instance ID (e.g., `ins-abc123xyz`)

The prompts are written in Korean but follow a structured format that can be adapted to other languages if needed.
