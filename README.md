# 0.0.0.0
Start here.
todo reserved_terms_map.yaml 
version: 0.1
scope: github_ui_to_canonical
matching:
  # Deterministic matching rules (apply in this order):
  # 1) exact string match
  # 2) exact match after whitespace normalization (collapse internal whitespace to single spaces)
  # 3) regex match (anchored)
  precedence: [exact, normalized_exact, regex]
  case_sensitivity: preserve  # do not casefold unless you explicitly add aliases
  normalize_whitespace: true
  trim: true

null_semantics:
  NULL: {meaning: "field intentionally empty / not provided"}
  UNSET: {meaning: "field exists but no user-selected value"}
  EMPTY_SET: {meaning: "field is a set and contains no items"}
  UNASSIGNED: {meaning: "assignee field explicitly empty"}

tokens:
  # Identity + Location
  - match: {type: exact, value: "Open"}
    emit: {token: VIEW.OPEN_ITEM}

  - match: {type: exact, value: "00-start-here"}
    emit: {token: ARTIFACT.START_HERE, attributes: {type: WORK_ITEM}}

  - match: {type: exact, value: "top-level-directory"}
    emit: {token: LOCATION.ROOT, attributes: {scope: REPO_OR_PROJECT_ROOT}}

  - match: {type: exact, value: "Private"}
    emit: {token: ACCESS.PRIVATE}

  - match: {type: exact, value: "Jump to bottom"}
    emit: {token: UI.JUMP_BOTTOM}

  # Description + Activity
  - match: {type: exact, value: "No description provided."}
    emit: {token: FIELD.DESCRIPTION, value: NULL}

  - match: {type: exact, value: "Activity"}
    emit: {token: AUDIT.ACTIVITY_LOG}

  - match: {type: regex, value: "^added this to .* project$"}
    emit: {token: EVENT.PROJECT_ITEM_ADDED}

  - match: {type: exact, value: "moved this to Todo"}
    emit: {token: EVENT.STATUS_CHANGED, attributes: {status: STATUS.TODO}}

  - match:
      type: regex
      value: "^(Add a comment|new Comment)$"
    emit: {token: ACTION.ADD_COMMENT}

  - match: {type: exact, value: "Write"}
    emit: {token: UI.MARKDOWN.EDIT_MODE}

  - match: {type: exact, value: "Preview"}
    emit: {token: UI.MARKDOWN.PREVIEW_MODE}

  # Ownership + Classification
  - match: {type: exact, value: "Assignees"}
    emit: {token: FIELD.ASSIGNEE}

  - match: {type: exact, value: "No one"}
    emit: {token: FIELD.ASSIGNEE, value: UNASSIGNED}

  - match: {type: exact, value: "Labels"}
    emit: {token: FIELD.LABELS}

  - match: {type: exact, value: "No labels"}
    emit: {token: FIELD.LABELS, value: EMPTY_SET}

  - match: {type: regex, value: "^Projects / @00-start-here’s untitled project$"}
    emit: {token: CONTAINER.PROJECT}

  - match: {type: exact, value: "Status / Todo"}
    emit: {token: FIELD.STATUS, value: STATUS.TODO}

  # Planning Fields (dropdowns)
  - match: {type: exact, value: "Team (Choose an option)"}
    emit: {token: FIELD.TEAM, value: UNSET}

  - match: {type: exact, value: "Priority (Choose an option)"}
    emit: {token: FIELD.PRIORITY, value: UNSET}

  - match: {type: exact, value: "Size (Choose an option)"}
    emit: {token: FIELD.SIZE, value: UNSET}

  - match: {type: exact, value: "Estimate (Enter number…)"}
    emit: {token: FIELD.ESTIMATE, value: UNSET, attributes: {type: NUMBER}}

  - match: {type: exact, value: "Iteration (Choose an iteration)"}
    emit: {token: FIELD.ITERATION, value: UNSET}

  - match: {type: exact, value: "Start date (No date)"}
    emit: {token: FIELD.START_DATE, value: NULL, attributes: {type: DATE}}

  - match: {type: exact, value: "Target date (No date)"}
    emit: {token: FIELD.TARGET_DATE, value: NULL, attributes: {type: DATE}}

  # Milestones + Relationships + Dev Links
  - match: {type: exact, value: "Milestone / No milestone"}
    emit: {token: FIELD.MILESTONE, value: NULL}

  - match: {type: exact, value: "Relationships / None yet"}
    emit: {token: FIELD.RELATIONSHIPS, value: EMPTY_SET}

  - match: {type: exact, value: "Development (link a pull request)"}
    emit: {token: FIELD.DEV_LINKS}

  - match: {type: exact, value: "link a pull request"}
    emit: {token: ACTION.LINK_PULL_REQUEST}

  # Notifications + Participants + Actions
  - match: {type: exact, value: "Notifications / Customize"}
    emit: {token: SETTINGS.NOTIFICATIONS}

  - match: {type: regex, value: "^You’re receiving notifications.*$"}
    emit: {token: STATE.SUBSCRIBED, value: true}

  - match: {type: exact, value: "Participants"}
    emit: {token: FIELD.PARTICIPANTS}

  - match: {type: exact, value: "Issue actions"}
    emit: {token: UI.ITEM_ACTIONS_MENU}

reserved_status:
  STATUS.TODO: {display: "Todo"}
  STATUS.IN_PROGRESS: {display: "In progress"}
  STATUS.DONE: {display: "Done"}
  STATUS.BLOCKED: {display: "Blocked", optional: true}

  todo jira_field_dictionary.yaml 
  version: 0.1
scope: canonical_to_jira
hierarchy:
  - INITIATIVE
  - PROJECT
  - CAPABILITY
  - EPIC
  - STORY_TASK
  - SUB_TASK

fields:
  FIELD.DESCRIPTION:
    jira_field: description
    type: text
    null_value: null

  FIELD.STATUS:
    jira_field: status
    type: enum
    allowed:
      - STATUS.TODO
      - STATUS.IN_PROGRESS
      - STATUS.DONE
      - STATUS.BLOCKED

  FIELD.ASSIGNEE:
    jira_field: assignee
    type: user
    empty_value: UNASSIGNED

  FIELD.LABELS:
    jira_field: labels
    type: set<string>
    empty_value: EMPTY_SET

  FIELD.TEAM:
    jira_field: customfield_TEAM
    type: enum
    unset_value: UNSET

  FIELD.PRIORITY:
    jira_field: priority
    type: enum
    unset_value: UNSET

  FIELD.SIZE:
    jira_field: customfield_SIZE
    type: enum
    unset_value: UNSET

  FIELD.ESTIMATE:
    jira_field: customfield_ESTIMATE
    type: number
    unset_value: UNSET

  FIELD.ITERATION:
    jira_field: customfield_ITERATION
    type: string
    unset_value: UNSET

  FIELD.START_DATE:
    jira_field: customfield_START_DATE
    type: date
    null_value: null

  FIELD.TARGET_DATE:
    jira_field: duedate
    type: date
    null_value: null

  FIELD.MILESTONE:
    jira_field: fixVersions   # or customfield_MILESTONE if you prefer
    type: set<string>
    null_value: null

  FIELD.RELATIONSHIPS:
    jira_field: issuelinks
    type: set<link>
    empty_value: EMPTY_SET

  FIELD.DEV_LINKS:
    jira_field: development
    type: set<link>
    empty_value: EMPTY_SET

events:
  EVENT.PROJECT_ITEM_ADDED:
    jira_event: issue_updated
    notes: "Translate as an issue update with project container metadata."

  EVENT.STATUS_CHANGED:
    jira_event: issue_transitioned
    attributes:
      status_from: {type: enum, optional: true}
      status_to: {type: enum, required: true}

actions:
  ACTION.ADD_COMMENT:
    jira_action: add_comment

  ACTION.LINK_PULL_REQUEST:
    jira_action: link_development
