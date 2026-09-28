2. Custom Command #2
Trigger Type: None (Executed via scheduleUniqueCC)

```gototemplate
{{/* --- VIP Role Removal CC (#39) --- */}}
{{ $data := .ExecData }}
{{ takeRoleID $data.userID $data.roleID }}
