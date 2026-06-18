# The Immutable Off‑Chain Backup
*Best for: servers where the owner is most at risk of account compromise.*

## Intro  
You’ve spent months hand‑crafting your Discord server: the perfect role hierarchy, emoji that spark joy, and a bot that plays sea shanties on command. 

#### A hacker doesn’t care.

They’ll delete it all in 11 seconds, then laugh in your DMs. 

The problem isn’t *if* your account gets compromised—it’s *when*. 

Most ‘backups’ are just copies the attacker can delete right after they delete your server. 

This article is for the kind of hardcore maniacs who spend 10 grand on a gaming PC.

It just so happens that I too, am a hardcore maniac. 

So let's be hardcore maniacs together, and secure your Discord server, nerd!

If you're reading this, I'm sure you understand that the only safe backup is one you **can’t** delete even if you tried. 

Spoiler: you’ll need a **Raspberry Pi**, a **YubiKey**, and a grudging respect for **AWS S3 Object Lock**.

I love checklists. So I've saved you hours of chaos, and built one just for you. Remember me in your prayers, fellow server owner.

### Step‑by‑Step Checklist
- [ ] **1. Get dedicated hardware** – Raspberry Pi 4 (or any always‑on old laptop/desktop) running Ubuntu or Raspberry Pi OS.
- [ ] **2. Create a separate Discord bot** – Go to Discord Developer Portal → New Application → Bot → Copy token.  
  *Permissions needed:* `Read Messages`, `Manage Channels`, `Manage Roles`, `Read Message History`.
- [ ] **3. Install backup software on the hardware** – SSH into the Pi and run:  
  `git clone https://github.com/tycrek/discord-backup.git` (example; use a maintained backup tool).  
  Or use `discord-chat-exporter` (CLI).
- [ ] **4. Set up immutable cloud storage** – Create an AWS S3 bucket → Enable **Object Lock** (governance or compliance mode).  
  *Region close to you, versioning ON, MFA Delete ON.*
- [ ] **5. Write backup script** – Daily cron job that:  
  - Exports server structure (roles, channels, perms) to JSON.  
  - Uploads JSON to S3 with `--object-lock-mode GOVERNANCE` flag.  
  - Deletes local copy after upload.
- [ ] **6. Store YubiKey offline** – The S3 bucket’s root credential is a hardware key (YubiKey 5 NFC) kept in a fire safe or second location.  
  *The Pi has no ability to delete the bucket – only write.*
- [ ] **7. Test recovery** – From a clean Discord account, use the bot to restore the JSON backup into a test server.

### ✅ Outcome
 Attacker compromises your main Discord account? They cannot touch the backup.
