# Windows Java Setup for adr.xyz Bot

## Step 1: Download and Install Oracle JDK 17

1. **Download Oracle JDK 17:**
   - Go to: https://www.oracle.com/java/technologies/downloads/#java17
   - Click "Download JDK 17"
   - Accept the license agreement
   - Download the Windows x64 Installer (usually `jdk-17_windows-x64_bin.exe`)

2. **Install JDK 17:**
   - Run the downloaded installer
   - Follow the installation wizard
   - Choose the default installation directory (usually `C:\Program Files\Java\jdk-17`)
   - Complete the installation

## Step 2: Set JAVA_HOME Environment Variable

1. **Open System Properties:**
   - Right-click on "This PC" or "My Computer"
   - Select "Properties"
   - Click "Advanced system settings" (on the right side)

2. **Edit Environment Variables:**
   - In the "System Properties" window, click "Environment Variables..."
   - Under "System variables", click "New..."

3. **Create JAVA_HOME Variable:**
   - **Variable name:** `JAVA_HOME`
   - **Variable value:** `C:\Program Files\Java\jdk-17`
   - Click "OK"

4. **Edit PATH Variable:**
   - In "System variables", find and select "Path"
   - Click "Edit..."
   - Click "New"
   - Add: `%JAVA_HOME%\bin`
   - Click "OK" to close all windows

## Step 3: Verify Installation

1. **Open Command Prompt:**
   - Press `Win + R`
   - Type `cmd`
   - Press Enter

2. **Check Java Installation:**
   ```cmd
   java -version
   ```
   You should see something like:
   ```
   java version "17.0.x"
   ```

3. **Check JAVA_HOME:**
   ```cmd
   echo %JAVA_HOME%
   ```
   You should see:
   ```
   C:\Program Files\Java\jdk-17
   ```

## Step 4: Install Additional Dependencies

### Install MongoDB:
1. Go to: https://www.mongodb.com/try/download/community
2. Download MongoDB Community Server for Windows
3. Run the installer and follow the setup

### Install Redis:
1. Download Redis for Windows from: https://github.com/microsoftarchive/redis/releases
2. Run the MSI installer
3. Redis will be installed as a Windows service

## Step 5: Build and Run adr.xyz

1. **Clone the repository:**
   ```cmd
   git clone https://github.com/tabledadriandev/adr.xyz.git
   cd adr.xyz
   ```

2. **Build the project:**
   ```cmd
   gradlew.bat shadowJar
   ```

3. **Run the bot:**
   ```cmd
   java -jar build/libs\adr.xyz-*.jar
   ```

## Troubleshooting

### If `gradlew.bat` is not recognized:
- Make sure you're in the correct directory
- Try running: `.\gradlew.bat shadowJar`

### If Java is still not found:
- Restart your Command Prompt after setting environment variables
- Check that JAVA_HOME points to the correct JDK directory

### If MongoDB/Redis connection fails:
- Make sure MongoDB and Redis services are running
- Check their default ports (MongoDB: 27017, Redis: 6379)

## Important Notes
- Keep the Command Prompt open while the bot is running
- The bot will create a `config.json` file on first run
- Edit `config.json` with your Discord bot token before the bot will work
- Press `Ctrl + C` in the Command Prompt to stop the bot

Your adr.xyz bot should now be ready to run! 🎉
