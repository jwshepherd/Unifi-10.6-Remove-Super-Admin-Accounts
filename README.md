# Unifi-10.6-Remove-Super-Admin-Accounts
Removing a super admin Account from a UOS Server 

Removing a SuperAdmin from a controller is a complete pain and I am not sure why the owner account doesn't even have the permissions to revoke or remove SuperAdmins.

But if you have ssh root access to the local server hosting it and need to remove and Superadmin user. you can use the following script.


<curl -sS http://127.0.0.1:9080/api/v2/users | python3 -c '
import json, sys
want = sys.argv[1].lower() if len(sys.argv) > 1 else ""
data = json.load(sys.stdin)
if isinstance(data, dict):
    data = data.get("data") or data.get("users") or []
for u in data:
    name = u.get("full_name") or u.get("username") or ""
    email = u.get("email") or ""
    user = u.get("username") or ""
    uid = u.get("unique_id") or u.get("id") or ""
    haystack = (name + " " + email + " " + user).lower()
    if want and want not in haystack:
        continue
    print(uid, name, email)
' james>
