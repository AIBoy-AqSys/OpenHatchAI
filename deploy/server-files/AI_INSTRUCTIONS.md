# AI Operational Rules for OpenHatchAI

## 1. Directory Scope Constraints
- **STRICT RULE**: Only access files within `./`.
- **FORBIDDEN**: Do NOT attempt to access parent directories, sibling directories, or any directory/file outside this project (e.g., `ls ..`, `cat /etc/...`).
- **FORBIDDEN**: Do NOT perform any operation (Reference, Confirm, Modify, Delete, Create) on files or directories above this project's path.
- **MANDATORY**: If you believe you need to access anything outside this directory, you MUST first inform the user of the exact purpose and obtain explicit confirmation before proceeding.
- **FORBIDDEN**: Do NOT search for "projects" or "source code" in the local host environment.
- **FORBIDDEN**: Do NOT run `npm install`, `docker-compose`, or any project-specific build/run commands unless they are confined to the `./` directory.

## 2. Project Purpose
- This directory is a **Server Construction Kit**.
- Its sole purpose is to define and maintain the manifests for building an **External VPS Server**.
- **NO LOCAL RELATION**: This local project has ZERO relationship with the actual application code beyond being its "blueprint" kit.
- Projects (Web games, etc.) do NOT exist here. They will be managed on the **Target VPS** in the future.

## 3. Mission
- Update the manifests in `INITIAL_MANIFEST/` to support:
    1. **Multi-project management**: Handling multiple git repositories on the VPS.
    2. **Browser Verification**: Ensuring projects can be viewed via a browser once deployed to the VPS.
- Always assume the VPS is the execution target for all application logic.

## 4. Contextual Reminders
- If you find yourself looking for local source code, **STOP**.
- Focus entirely on the `.md` manifest files and `README.txt`.
- Your work is to refine the **Server Blueprint**, not to develop the app locally.

## 5. Work Log Rules
- **STRICT RULE**: Always keep a work log of your activities.
- **Log File Path**: `log/[YYYYmmdd]_[Alphanumeric abbreviation].log`
- **MANDATORY**: Creating/Opening a log file MUST be the **VERY FIRST ACTION** of any task.
- **MANDATORY**: You must record the following in the log in real-time:
    - **Purpose**: What is the goal of this task?
    - **Thoughts**: Your reasoning and plan.
    - **Actions**: Exact commands run, files modified.
    - **Results**: Output of commands, verification results, and any issues encountered.
- **FORBIDDEN**: Do NOT wait until the end of the task to write the log. Update it progressively as you work.
## 6. Security Protocol
- **PERIODIC CHECK**: Regularly check for security improvements and defend against unauthorized operations/access.
- **CREDENTIALS CHECK**: Strictly ensure that API tokens, keys, passwords, and access information are NOT exposed in any files.
- **BACKDOOR CHECK**: Monitor for unintended access points or backdoors.
- **SECURITY LOG**: Maintain a `SECURITY.log` file at the project root for tracking security checks.
- **DAILY CHECKS**: 
    - Perform exactly 3 security checks from the `SECURITY.log` list daily.
    - Choose items that are either undated or have the oldest timestamps.
    - Record the date, target, result, and any problems found.
- **PRIORITY FIXING**:
    - If a security risk is discovered, it MUST be addressed at the very beginning of the next work session.
    - Proactive notification: "まず〇〇のセキュリティリスクに対処します" (First, I will address the security risk of XXX) must be stated.

## 7. Server-Side AI Guardrails
- **SERVER-SIDE DEPLOYMENT**: Similar security protocols are enforced on the server-side AI agent via `12_SERVER_AI_INSTRUCTIONS.md` in the Initial Manifest.
- **MANIFEST SYNC**: When updating security rules locally, ensure the server-side manifests are updated accordingly to maintain consistent security posture across all agents.
