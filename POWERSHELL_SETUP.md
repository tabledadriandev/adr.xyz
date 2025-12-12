# PowerShell Setup Script for adr.xyz Bot on Windows

## Step 1: Set Java Environment Variables (PowerShell)

Run these commands in PowerShell (Run as Administrator recommended):

```powershell
# Find Java installation path
$javaPath = Get-ChildItem -Path "C:\Program Files\Java" -Directory | Where-Object { $_.Name -match "^jdk" } | Sort-Object Name -Descending | Select-Object -First 1

# Set JAVA_HOME
[Environment]::SetEnvironmentVariable("JAVA_HOME", $javaPath.FullName, "Machine")

# Add Java to PATH
$currentPath = [Environment]::GetEnvironmentVariable("PATH", "Machine")
$javaBinPath = "$($javaPath.FullName)\bin"
if ($currentPath -notlike "*$javaBinPath*") {
    [Environment]::SetEnvironmentVariable("PATH", "$currentPath;$javaBinPath", "Machine")
}

Write-Host "Java environment variables set successfully!"
Write-Host "JAVA_HOME: $javaPath.FullName"
```

## Step 2: Install Chocolatey (Package Manager)

```powershell
# Install Chocolatey
Set-ExecutionPolicy Bypass -Scope Process -Force
[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072
iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```

## Step 3: Install MongoDB via PowerShell

```powershell
# Install MongoDB using Chocolatey
choco install mongod -y

# Create data directory
New-Item -ItemType Directory -Force -Path "C:\data\db"

# Start MongoDB service
Start-Service -Name "MongoDB"
Set-Service -Name "MongoDB" -StartupType Automatic
Write-Host "MongoDB installed and started successfully!"
```

## Step 4: Install Redis via PowerShell

```powershell
# Install Redis using Chocolatey
choco install redis-64 -y

# Start Redis service
Start-Service -Name "Redis"
Set-Service -Name "Redis" -StartupType Automatic
Write-Host "Redis installed and started successfully!"
```

## Step 5: Verify Java Installation

```powershell
# Verify Java installation
java -version
javac -version

# Verify JAVA_HOME
echo $env:JAVA_HOME

# If JAVA_HOME is not showing, restart PowerShell or run:
$env:JAVA_HOME = [Environment]::GetEnvironmentVariable("JAVA_HOME", "Machine")
```

## Step 6: Clone and Build adr.xyz

```powershell
# Clone the repository
git clone https://github.com/tabledadriandev/adr.xyz.git
cd adr.xyz

# Build the project
.\gradlew.bat shadowJar

# Run the bot
java -jar build\libs\adr.xyz-*.jar
```

## Complete One-Liner Setup (Copy-Paste)

```powershell
# Complete setup - Copy and paste this entire block
Write-Host "Setting up adr.xyz Bot environment..." -ForegroundColor Green

# Set Java environment variables
$javaPath = Get-ChildItem -Path "C:\Program Files\Java" -Directory | Where-Object { $_.Name -match "^jdk" } | Sort-Object Name -Descending | Select-Object -First 1
[Environment]::SetEnvironmentVariable("JAVA_HOME", $javaPath.FullName, "Machine")
$currentPath = [Environment]::GetEnvironmentVariable("PATH", "Machine")
$javaBinPath = "$($javaPath.FullName)\bin"
if ($currentPath -notlike "*$javaBinPath*") {
    [Environment]::SetEnvironmentVariable("PATH", "$currentPath;$javaBinPath", "Machine")
}

# Install Chocolatey
Set-ExecutionPolicy Bypass -Scope Process -Force
[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072
iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))

# Install MongoDB
choco install mongod -y
New-Item -ItemType Directory -Force -Path "C:\data\db"
Start-Service -Name "MongoDB" -ErrorAction SilentlyContinue
Set-Service -Name "MongoDB" -StartupType Automatic -ErrorAction SilentlyContinue

# Install Redis
choco install redis-64 -y
Start-Service -Name "Redis" -ErrorAction SilentlyContinue
Set-Service -Name "Redis" -StartupType Automatic -ErrorAction SilentlyContinue

Write-Host "Setup completed! Restart PowerShell and run the build commands." -ForegroundColor Green
```

## After Setup - Build and Run

```powershell
# In a new PowerShell window (after restart):
git clone https://github.com/tabledadriandev/adr.xyz.git
cd adr.xyz
.\gradlew.bat shadowJar
java -jar build\libs\adr.xyz-*.jar
```

## Troubleshooting

### If Java version is wrong:
```powershell
# Check Java version
java -version

# If you need to use a specific Java version, set JAVA_HOME manually:
$env:JAVA_HOME = "C:\Program Files\Java\jdk-25"
$env:PATH += ";$env:JAVA_HOME\bin"
```

### If services don't start:
```powershell
# Check service status
Get-Service -Name "MongoDB"
Get-Service -Name "Redis"

# Start services manually
Start-Service -Name "MongoDB"
Start-Service -Name "Redis"
```

### If gradlew is not found:
```powershell
# Make sure you're in the right directory
Get-Location
dir

# Run gradlew with explicit path
.\gradlew.bat shadowJar
```

Your adr.xyz bot should now be ready to run! 🎉
