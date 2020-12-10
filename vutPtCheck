#!/usr/bin/python3
import json
import urllib
import urllib.request
import getpass
import re

user = input("Username: ")
passwd = getpass.getpass()

# create a password manager
password_mgr = urllib.request.HTTPPasswordMgrWithDefaultRealm()

# If we knew the realm, we could use it instead of None.
top_level_url = "https://wis.fit.vutbr.cz/FIT/st"
password_mgr.add_password(None, top_level_url, user, passwd)

handler = urllib.request.HTTPBasicAuthHandler(password_mgr)

# create "opener" (OpenerDirector instance)
opener = urllib.request.build_opener(handler)

# use the opener to fetch a URL
try:
    opener.open("https://wis.fit.vutbr.cz/FIT/st/get-coursesx.php?json=1&sem=Z")
except:
    print("Bad username || passord")
    exit(1);

# Install the opener.
# Now all calls to urllib.request.urlopen use our opener.
urllib.request.install_opener(opener)
r = urllib.request.urlopen("https://wis.fit.vutbr.cz/FIT/st/get-coursesx.php?json=1&sem=Z")

data = r.read()
data = data.decode('utf-8')
data = json.loads(data)
data = data["data"]

for i in data:
    print(f'{i["abbrv"]}:')
    for f in i["items"]:
        if f["type"] == "credit":
            print(f'    [i]Points: {i["points"]}')
            print(f'    [i]Min points needed to pass: {f["min"]}')

        if re.match(r".*projekt.*", f["title"], re.IGNORECASE):
            if not re.match(r".*předchoz.*", f["title"], re.IGNORECASE):
                print(f"    [!]Potencional requirement: {f['title']}")
