## Moltbot

### Step 1: Installation

```bash
sudo apt install python3 python3-pip python3-venv discord.py
python3 -m venv ~/moltbot
source ~/moltbot/bin/activate
pip install discord.py
```

### Step 2: Sample code

```bash
import discord
client = discord.Client(intents=discord.Intents.default())

@client.event
async def on_ready():
    print(f'{client.user} online!')

client.run('YOUR_TOKEN')
```

### Step 3: Docker Setup

```bash
docker run -d -v ~/moltbot:/app python:3.12 python /app/molt.py
```