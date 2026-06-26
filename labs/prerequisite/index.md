---
layout: carbon
title: "Prerequisites"
---

# Prerequisites — AI Government Expense Tracker

Complete prerequisites guide for the AI Government Expense Tracker lab.

---

## System Requirements

- **Operating System**: macOS, Linux, or Windows
- **Python Version**: 3.10, 3.11, 3.12, or 3.13 (recommended)
  - Python 3.14 requires special setup (see below)
- **Internet Connection**: Required for first run (downloads ~2GB of ML models)
- **Disk Space**: At least 5GB free

---

## Installing Python

If you don't have Python installed, follow these steps:

**Mac:**
1. Go to [python.org/downloads](https://python.org/downloads).
2. Click **Download Python 3.13.x** (the big yellow button).
3. Open the downloaded `.pkg` file and follow the installer.
4. When done, open Terminal and run `python3 --version` to confirm.

**Windows**:
1. Go to [python.org/downloads](https://python.org/downloads, Download and install Python Install Manager.
2. Open Python Install Manager when installation is complete.
3. In the Python Install Manager window, run:

   ```bash
   py install 3.13
   ```

4. Wait for Python 3.13 to finish installing.
5. Open Command Prompt and run:

   ```bash
   py -3.13 --version
   ```

6. If the command is not recognized, add Python to your PATH:
   - Open **Edit the system environment variables**.
   - Click **Environment Variables**.
   - Edit the **Path** variable.
   - Add the Python installation directory and Scripts directory.
   - restart your device.
7. Open a new Command Prompt and run:

   ```bash
   py -3.13 --version
   ```

   to confirm the installation

> **Note for Python 3.14 users:** If you already have Python 3.14 installed, you must still install Python 3.13 using the steps above. When running scripts, always use:

```bash
py -3.13 YOURFILE.py
```

instead of:

```bash
python YOURFILE.py
```

or:

```bash
py YOURFILE.py
```

to ensure Python 3.13 is used.

> 💡 pip comes bundled with Python 3.13. If `pip3 --version` gives an error,
> run `python3 -m ensurepip` to install it.

---

## Installing IBM Bob

1. Search for IBM Bob online.
2. Download the IBM Bob installer for your operating system.
3. Open the installer and follow the prompts.
4. Launch Bob — you should see the Bob IDE with the chat panel on the right.

---

## Get the Workshop Materials

You'll need the sample invoices, reference solution, and credential template from the
workshop repository. Choose **one** of the options below.

### Option A — Clone with Git (recommended)

```bash
git clone https://github.com/Austinkkk3/Bob-Dev-Day-Track-C-Austin.git
cd Bob-Dev-Day-Track-C-Austin
```

### Option B — Download the ZIP

1. Open the repository in your browser:
   **[github.com/Austinkkk3/Bob-Dev-Day-Track-C-Austin](https://github.com/Austinkkk3/Bob-Dev-Day-Track-C-Austin)**
2. Click the green **`< > Code`** button, then **Download ZIP**.
3. Unzip the file — you'll get a folder named `Bob-Dev-Day-Track-C-Austin-main`.
4. Open that folder in your terminal:

   ```bash
   cd ~/Downloads/Bob-Dev-Day-Track-C-Austin-main
   ```

> 💡 Inside the folder you'll find `Sample-Invoices/` (test PDFs), `solution/`
> (reference implementation if you get stuck), `env.txt` (credentials template),
> and `requirements.txt`.

---

## Required Accounts

To access and use IBM watsonx products for this Bobathon, participants must create an
IBMid to log into your IBM Cloud account. This account will provide the necessary environment to work with the supporting IBM products for this hackathon.

### Creating an IBMid

You must have an IBMid to sign into IBM Cloud. With an IBMid, you can use your email login
 to log into all IBM products and services. If you don't have an existing
IBMid, follow the procedure below to create one.

1. Create an IBMid by accessing the [Create your IBM account](https://www.ibm.com/account/reg/us-en/signup?formid=urx-19776&target=https%3A%2F%2Flogin.ibm.com%2Foidc%2Fendpoint%2Fdefault%2Fauthorize%3FqsId%3Db3f11bc3-60d2-45e8-82ae-8bc09a47bcc7%26client_id%3DMyIBMLondonProdCI) page.
2. Enter all required information in the fields provided. 
3. Select Next. You will receive an email from IBM Security that contains a one-time
verification code.
4. In the verification token field, enter the code that is provided in the email.
5. Click submit.
6. An email will be sent to you from IBM Security indicating that your IBMid account
creation was successful and that your account is activated.

### IBM Cloud Account

**IBM Cloud Account** (free tier available) — sign up at [cloud.ibm.com](https://cloud.ibm.com)

### Accessing and utilizing IBM watsonx products

Once you and your team are assigned Cloud environments, be sure to accept the environment
cluster invitation that will be sent to your email. It should look something like this:

<img width="599" height="590" alt="Screenshot 2026-06-11 at 10 29 03 AM" src="https://github.com/user-attachments/assets/3bd4a682-d20c-4852-bde0-aaaa96d80b8f" />


**IMPORTANT: You must click the "Join now" to get access, and ensure you log into IBM Cloud**
using the same email you used for the IBMid setup. Not clicking the join now button or using a
different email will not give you proper access to the environments!

To begin building your solution, explore the capabilities and resources for each IBM watsonx
product enabled for this hackathon.

---

## Get Your Credentials

Before writing any code, you need three pieces of information from IBM Cloud.

### Step 1: Get IBM Cloud API Key

1. Go to [cloud.ibm.com](https://cloud.ibm.com) and sign in.
2. Click on the highlighted icon with the arrow. If you expand the display with the hamburger menu
icon on the top-left, you will see this is the Resource list

 <img width="1009" height="541" alt="Screenshot 2026-06-11 at 10 31 32 AM" src="https://github.com/user-attachments/assets/ab27273b-abdf-44d9-8790-44c728d283da" />

4. Click on the down arrow beside AI/Machine Learning. The window will expand to show multiple
resources. Click the resource name with the product watsonx.ai Runtime.
<img width="1004" height="647" alt="Screenshot 2026-06-11 at 10 32 28 AM" src="https://github.com/user-attachments/assets/8f4d9a83-6798-494e-83b3-4cfe408360b4" />


 or
 
<img width="1001" height="542" alt="Screenshot 2026-06-11 at 10 33 20 AM" src="https://github.com/user-attachments/assets/fc1b9117-5329-41b0-a1b2-e700cb62a593" />


4. Launch in IBM WatsonX
<img width="1015" height="542" alt="Screenshot 2026-06-11 at 10 33 48 AM" src="https://github.com/user-attachments/assets/cedde2d9-06b8-49f8-a966-08729f286587" />


5. Scroll to find the **Projects** tab.
6. <img width="980" height="522" alt="Screenshot 2026-06-11 at 10 36 56 AM" src="https://github.com/user-attachments/assets/967977c9-46ab-46bb-a346-a1735a1921f1" />



7. Create a new project and give it a name, then click **Create**.
8. Scroll down to find **Storage Service** (Pick the same one as your resource name)
9. <img width="980" height="522" alt="Screenshot 2026-06-11 at 10 37 22 AM" src="https://github.com/user-attachments/assets/2fde456d-9e9f-4e52-bab8-9fcd98dc7c5b" />


10. Navigate to **Manage** and copy the **Project ID**, then save it in your notepad.
11. In the same Manage page, click on **Services and Integration**.
12. <img width="1512" height="812" alt="Untitled3" src="https://github.com/user-attachments/assets/22a7873a-1a77-4c9f-ab93-2bd14284ce09" />


13. Click **Associate Service**, check the **watsonx Runtime** instance (Pick the same one as your resouce name), and confirm.
14.<img width="1512" height="812" alt="Untitled4" src="https://github.com/user-attachments/assets/f1827dbc-80fd-48af-bb7e-2cf0b0a094dc" />


15. Click the three lines (hamburger menu) and click **Home**.
16. Find the **Developer Access** section
<img width="1005" height="538" alt="Screenshot 2026-06-11 at 10 39 14 AM" src="https://github.com/user-attachments/assets/b47b4875-7085-4e39-a4ca-d760850dcdf3" />


17. Select your project from the dropdown.
18. Click **Create API key** and give it a name (e.g., `watsonx-expense-tracker`).

19. ⚠️ **Copy the API key immediately** — it's shown only once.
20. Save it securely (you'll paste it into `.env` later).

 




### Step 2: Note Your Region

Your `CLOUD_URL` depends on your IBM Cloud region:

| Region | URL |
|--------|-----|
| US South (Dallas) | `https://us-south.ml.cloud.ibm.com` |
| Canada (Toronto) | `https://ca-tor.ml.cloud.ibm.com` |
| Europe (Frankfurt) | `https://eu-de.ml.cloud.ibm.com` |
| UK (London) | `https://eu-gb.ml.cloud.ibm.com` |

Check your region in the IBM Cloud dashboard (top right, next to your account name).

---

## Ready to Start

Once you have completed all the prerequisites above, you're ready to begin building your AI Government Expense Tracker!

👉 **[Continue to LAB1.md →](/ottawa-bob-dev-day/labs/lab-1-build/)**