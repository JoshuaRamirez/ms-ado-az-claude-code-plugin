---
description: Assign areas and iterations to a team
allowed-tools: Bash(az boards area:*), Bash(az boards iteration:*), Bash(az devops invoke:*)
---

# Assign Areas/Iterations to Team

Configure which areas and iterations a team owns.

## View Team's Current Areas

```
az boards area team list --team {TEAM} -o table
```

## Add Area to Team

```
az boards area team add --team {TEAM} --path "{AREA_PATH}"
```

Include sub-areas:
```
az boards area team add --team {TEAM} --path "{AREA_PATH}" --include-sub-areas true
```

## Set Default Area for Team

```
az boards area team update --team {TEAM} --path "{AREA_PATH}" --set-as-default
```

## Remove Area from Team

```
az boards area team remove --team {TEAM} --path "{AREA_PATH}"
```

---

## View Team's Current Iterations

```
az boards iteration team list --team {TEAM} -o table
```

## Add Iteration to Team

```
az boards iteration team add --team {TEAM} --id {ITERATION_ID}
```

Or by path:
```
az boards iteration team add --team {TEAM} --path "{ITERATION_PATH}"
```

## Remove Iteration from Team

```
az boards iteration team remove --team {TEAM} --id {ITERATION_ID}
```

## Set Backlog Iteration

The root iteration for the team's backlog:
```
az boards iteration team set-backlog-iteration --team {TEAM} --id {ITERATION_ID}
```

## Set Default Iteration

New work items default to this iteration:
```
az boards iteration team set-default-iteration --team {TEAM} --id {ITERATION_ID}
```

---

## Get Team Settings (All Config)

For complete team configuration via API:
```
az devops invoke \
  --area work \
  --resource teamsettings \
  --route-parameters project={PROJECT} team={TEAM} \
  --api-version 7.1 \
  -o json
```

Shows:
- backlogIteration
- defaultIteration
- bugsBehavior (Off, AsRequirements, AsTasks)
- workingDays
- backlogVisibilities
