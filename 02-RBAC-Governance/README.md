# Project 2: [RBAC & Governance] 🚧 

## Objective 🚧 
One or two sentences: what problem this solves / what capability it demonstrates.

## AZ-104 Skills Covered 🚧 
- Bullet list mapped to the official exam skills outline

## Architecture 🚧 
[diagram image]
Brief explanation of the design and any trade-offs made.

## What I Built 🚧 
Step-by-step narrative — explain *why*, not just *what*.

## Security/Design Decisions 🚧 


## Challenges & What I'd Do Differently 🚧 
Genuine reflection

## Cost 🚧 
What this cost to run, and how you kept it near-zero.

## Screenshots / Diagrams 🚧 
(redact tenant IDs, subscription IDs, and any real emails/usernames)

### Custom Role JSON (WITHOUT wildcard)
```JSON
{
    "properties": {
        "roleName": "Storage Auditor",
        "description": "Can view storage account configuration, network rules, and diagnostic settings, but cannot modify, delete, or access data plane (blob/file/queue/table contents).",
        "assignableScopes": [
            "/subscriptions/subscription-id"
        ],
        "permissions": [
            {
                "actions": [
                    "Microsoft.Storage/storageAccounts/read",
                    "Microsoft.Storage/storageAccounts/blobServices/read",
                    "Microsoft.Storage/storageAccounts/fileServices/read",
                    "Microsoft.Storage/storageAccounts/queueServices/read",
                    "Microsoft.Storage/storageAccounts/tableServices/read",
                    "Microsoft.Storage/storageAccounts/privateEndpointConnections/read",
                    "Microsoft.Authorization/acquirePolicyToken/read",
                    "Microsoft.Authorization/denyAssignments/read",
                    "Microsoft.Authorization/diagnosticSettingsCategories/read",
                    "Microsoft.Authorization/diagnosticSettings/read",
                    "Microsoft.Authorization/roleEligibilityScheduleInstances/read",
                    "Microsoft.Authorization/locks/read",
                    "Microsoft.Authorization/operations/read",
                    "Microsoft.Authorization/permissions/read",
                    "Microsoft.Authorization/policyAssignments/read",
                    "Microsoft.Authorization/policyAssignments/privateLinkAssociations/read",
                    "Microsoft.Authorization/policyAssignments/resourceManagementPrivateLinks/read",
                    "Microsoft.Authorization/policyAssignments/resourceManagementPrivateLinks/privateEndpointConnections/read",
                    "Microsoft.Authorization/policyAssignments/resourceManagementPrivateLinks/privateEndpointConnectionProxies/read",
                    "Microsoft.Authorization/policyDefinitions/read",
                    "Microsoft.Authorization/policyDefinitions/versions/read",
                    "Microsoft.Authorization/policyEnrollments/read",
                    "Microsoft.Authorization/policyExemptions/read",
                    "Microsoft.Authorization/policySetDefinitions/read",
                    "Microsoft.Authorization/policySetDefinitions/versions/read",
                    "Microsoft.Authorization/providerOperations/read",
                    "Microsoft.Authorization/roleAssignments/read",
                    "Microsoft.Authorization/roleAssignmentSchedules/read",
                    "Microsoft.Authorization/roleAssignmentScheduleInstances/read",
                    "Microsoft.Authorization/roleAssignmentScheduleRequests/read",
                    "Microsoft.Authorization/roleDefinitions/read",
                    "Microsoft.Authorization/roleEligibilitySchedules/read",
                    "Microsoft.Authorization/roleEligibilityScheduleRequests/read",
                    "Microsoft.Authorization/roleManagementPolicies/read",
                    "Microsoft.Authorization/roleManagementPolicyAssignments/read",
                    "Microsoft.Resources/subscriptions/resourceGroups/read",
                    "Microsoft.Insights/DiagnosticSettings/Read"
                ],
                "notActions": [
                    "Microsoft.Storage/storageAccounts/listkeys/action"
                ],
                "dataActions": [],
                "notDataActions": [
                    "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read",
                    "Microsoft.Storage/storageAccounts/fileServices/fileshares/files/read"
                ]
            }
        ]
    }
}
```

### Custom Role JSON (WITH wildcard)
Adding the (*) wildcard shortens the JSON by 29 lines
```JSON
{
    "properties": {
        "roleName": "Storage Auditor",
        "description": "Can view storage account configuration, network rules, and diagnostic settings, but cannot modify, delete, or access data plane (blob/file/queue/table contents).",
        "assignableScopes": [
            "/subscriptions/subscription-id"
        ],
        "permissions": [
            {
                "actions": [
                    "Microsoft.Storage/storageAccounts/read",
                    "Microsoft.Storage/storageAccounts/blobServices/read",
                    "Microsoft.Storage/storageAccounts/fileServices/read",
                    "Microsoft.Storage/storageAccounts/queueServices/read",
                    "Microsoft.Storage/storageAccounts/tableServices/read",
                    "Microsoft.Storage/storageAccounts/privateEndpointConnections/read",
               "Microsoft.Authorization/*/read",
                    "Microsoft.Resources/subscriptions/resourceGroups/read",
                    "Microsoft.Insights/DiagnosticSettings/Read"
                ],
                "notActions": [
                    "Microsoft.Storage/storageAccounts/listkeys/action"
                ],
                "dataActions": [],
                "notDataActions": [
                    "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read",
                    "Microsoft.Storage/storageAccounts/fileServices/fileshares/files/read"
                ]
            }
        ]
    }
}
```
