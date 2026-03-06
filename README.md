# Firefox Container

A disposable Firefox browser running in Docker for safe suspicious link testing.

## Why I Built This

Why I Built This
This project started with a simple question: "Can I run Kali Linux in Docker?" I was curious about Docker's capabilities and wanted to see if I could use it for security practice. After experimenting with Kali containers, I realized that a web browser was actually a more practical and focused use case for container isolation. The Firefox sandbox lets me safely test suspicious links without risking my main Windows machine, turning curiosity about Docker into a useful security tool.

## How It Works

Docker acts as a middleman between the operating system and the application running inside a container. When an application needs resources (CPU, memory, network), Docker requests them directly from the host OS. This makes containers lightweight and scalable—they only use what they need at the moment. This is different from a Virtual Machine (VM). A VM uses a hypervisor to simulate entire hardware, then runs a full guest OS on top of it. That means more overhead and dedicated resources that can't be easily shared. Because a container runs as a single application (like Firefox) rather than a full operating system, it can scale up or down based on demand. Docker handles the resource requests dynamically, so the container stays efficient. For this project, Firefox runs isolated inside the container. All its browsing activity—cookies, cache, even potential malware—stays inside the container and disappears when the container is removed.

## Firefox Sandbox in Action

<img width="700" alt="Firefox sandbox running VirusTotal" src="https://github.com/user-attachments/assets/345369dd-ba6b-4cfb-9745-185b984079e9" />
<img width="1268" height="600" alt="DockerFirefoxSandbox" src="https://github.com/user-attachments/assets/b85ce035-c306-44bf-88fd-9a169ce47b21" />



## Tools Used

- Docker Desktop - For running and managing containers on Windows 11
- WSL 2 - Windows Subsystem for Linux backend for Docker
- jlesage/firefox - Docker image with Firefox and VNC web access
- PowerShell - For running Docker commands
- Windows 11 - Host operating system
- AI assistant - For step-by-step guidance and troubleshooting during development

## Steps I Took

1) Used AI to narrow the project scope - I asked what would be the best way to create a security sandbox, and through questions, defined the project's function as an isolated browser for safe link testing.
2) Installed Docker Desktop on Windows 11 (setup guidance provided by AI).
3) Discovered the jlesage/firefox image through AI recommendations—it was a better fit for isolated browsing than my initial Kali Linux experiments.
4) Ran the Firefox container using: docker run -d --name=firefox-sandbox -p 5800:5800 jlesage/firefox (Note: The port mapping X:X can be adjusted, I used 5800:5800 for this project.)
5) After waiting a few minutes, accessed Firefox at http://localhost:5800.
6) Learned container management - When done, I could stop the sandbox using: docker stop firefox-sandbox (This can also be done through the Docker Desktop application's stop button.)
7) Discovered an important lesson - If I tried to run a new container with the same name without removing the old one, Docker gave an error that the container already exists. Instead of creating a new one, I could restart the existing container with: docker start firefox-sandbox
8) Removed the container when I wanted a completely fresh start: docker rm firefox-sandbox, This allows me to run step 4 again for a brand new, clean sandbox next time.

## What I Learned

1) Containers provide strong isolation - Running a single application (like Firefox) in a container is an ideal use case. Even if the container gets contaminated by a malicious site, the host system remains safe because the container is isolated. This makes containers perfect for disposable testing environments, I can use Firefox, then destroy everything with no traces left behind.
2) Dynamic resource allocation - Unlike a VM, containers don't need resources allocated before startup. Docker dynamically pulls resources from the host based on what the application needs at any given moment. This efficiency means I can run multiple containers on the same hardware without planning resource allocation ahead of time.
3) Docker Desktop as a management tool - Docker Desktop isn't just for running containers—it's also a management interface. You can inspect container details, view logs, and control containers through the application. Commands like docker ps, docker logs, and docker inspect give me visibility into what's happening inside containers.
4) Port mapping connects isolation to usability - The -p 5800:5800 flag maps a port inside the isolated container to my host machine. This is what lets me actually use Firefox in my regular browser at http://localhost:5800 while keeping the container secure.
5) WSL 2 as the backend - On Windows, Docker Desktop uses WSL 2 (Windows Subsystem for Linux) as its backend infrastructure. This allows Linux containers to run seamlessly on Windows by providing the necessary Linux kernel compatibility.
6) Containers persist until removed - A container doesn't disappear when it stops. It exists on your system until you explicitly remove it with docker rm. This is why I encountered the "container name already in use" error, the old container was still there even though it wasn't running. The full container lifecycle is: run → stop → start → rm, with each command serving a specific purpose.
