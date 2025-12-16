TODO- replace with unique start (in-dev) recusrion. 

Proto English → Reserved Terms Map

Identity + Location
	•	“Open” → VIEW.OPEN_ITEM
	•	“00-start-here” → ARTIFACT.START_HERE (type: WORK_ITEM)
	•	“top-level-directory” → LOCATION.ROOT (scope: REPO_OR_PROJECT_ROOT)
	•	“Private” → ACCESS.PRIVATE
	•	“Jump to bottom” → UI.JUMP_BOTTOM

Description + Activity
	•	“No description provided.” → FIELD.DESCRIPTION = NULL
	•	“Activity” → AUDIT.ACTIVITY_LOG
	•	“added this to … project” → EVENT.PROJECT_ITEM_ADDED
	•	“moved this to Todo” → EVENT.STATUS_CHANGED with STATUS.TODO
	•	“Add a comment / new Comment” → ACTION.ADD_COMMENT
	•	“Write / Preview” → UI.MARKDOWN.EDIT_MODE / UI.MARKDOWN.PREVIEW_MODE

Ownership + Classification
	•	“Assignees / No one” → FIELD.ASSIGNEE = UNASSIGNED
	•	“Labels / No labels” → FIELD.LABELS = EMPTY_SET
	•	“Projects / @00-start-here’s untitled project” → CONTAINER.PROJECT
	•	“Status / Todo” → FIELD.STATUS = STATUS.TODO

Planning Fields (the dropdowns)
	•	“Team (Choose an option)” → FIELD.TEAM = UNSET
	•	“Priority (Choose an option)” → FIELD.PRIORITY = UNSET
	•	“Size (Choose an option)” → FIELD.SIZE = UNSET
	•	“Estimate (Enter number…)” → FIELD.ESTIMATE = UNSET (type: NUMBER)
	•	“Iteration (Choose an iteration)” → FIELD.ITERATION = UNSET
	•	“Start date (No date)” → FIELD.START_DATE = NULL (type: DATE)
	•	“Target date (No date)” → FIELD.TARGET_DATE = NULL (type: DATE)

Milestones + Relationships + Dev Links
	•	“Milestone / No milestone” → FIELD.MILESTONE = NULL
	•	“Relationships / None yet” → FIELD.RELATIONSHIPS = EMPTY_SET
	•	“Development (link a pull request)” → FIELD.DEV_LINKS / ACTION.LINK_PULL_REQUEST

Notifications + Participants + Actions
	•	“Notifications / Customize” → SETTINGS.NOTIFICATIONS
	•	“You’re receiving notifications…” → STATE.SUBSCRIBED = TRUE
	•	“Participants” → FIELD.PARTICIPANTS
	•	“Issue actions” → UI.ITEM_ACTIONS_MENU

⸻

Minimal “Reserved Status” Set (recommended)
	•	“Todo” → STATUS.TODO
	•	(future) “In progress” → STATUS.IN_PROGRESS
	•	(future) “Done” → STATUS.DONE
	•	(future) “Blocked” → STATUS.BLOCKED (optional)

⸻

If you want this to be Jira-compatible, I can output the same map as a Jira Field Dictionary (Issue Fields + allowed values) using your preferred hierarchy: Initiative → Project → Capability → Epic → Story/Task → Sub-task.
