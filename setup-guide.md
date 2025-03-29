# Setup Guide for EVE-NG on GCP

This guide provides step-by-step instructions on setting up **EVE-NG** on **Google Cloud Platform (GCP)**.

## Prerequisites
Before starting, ensure you have the following:
- A **Google Cloud** account: If you don't have one, create it at [cloud.google.com](https://cloud.google.com/).
- Basic understanding of **Google Cloud's Compute Engine** and **SSH**.
- A modern browser for accessing **EVE-NG** once it’s set up.

## Step 1: Prepare the Ubuntu Boot Disk Template

In this step, we will prepare a **nested Ubuntu 22.04 image** for your instance. This will allow you to use the Ubuntu operating system for network emulation within EVE-NG.

### Instructions:
1. **Open the Google Cloud Shell**:
   - On the Google Cloud Console, click on the **Activate Cloud Shell** button at the top-right corner of the page (just below your project name). This will open a terminal window at the bottom of your browser.
   - Click **START CLOUD SHELL** to initialize it.

2. **Create the Nested Ubuntu 22.04 Image**:
   - Copy and paste the following command into the Cloud Shell. This command will create an Ubuntu 22.04 image that supports nested virtualization.

     **Command:**
     ```bash
     gcloud compute images create nested-ubuntu-jammy --source-image-family=ubuntu-2204-lts --source-image-project=ubuntu-os-cloud --licenses https://www.googleapis.com/compute/v1/projects/vmoptions/global/licenses/enable-vmx
     ```

   - After pasting, press **Enter** to execute the command.

3. **Confirmation**:
   - You should see a confirmation message that the image is being created. Once it's ready, you'll be able to use it for your VM instances.

By completing these steps, you’ve created a boot disk template for Ubuntu 22.04 that is compatible with EVE-NG.

## Step 2: Create a Google Cloud Project
1. Log in to [Google Cloud Console](https://console.cloud.google.com/).
2. Create a new project by selecting the **Project Dropdown** at the top of the page and clicking **New Project**.
3. Name your project and click **Create**.

## Step 3: Set Up a Compute Engine VM
1. In the **Google Cloud Console**, navigate to **Compute Engine** > **VM Instances**.
2. Click **Create Instance**.
3. Configure the instance:
   - **Name**: `eve-ng-instance`
   - **Region and Zone**: Select your desired region and zone.
   - **Machine Type**: Choose a machine type. `n1-standard-2` is recommended for a balance of performance and cost.
   - **Boot Disk**: Select Custom images, select nested-ubuntu-jammy you created previously
   - **Firewall**: Allow HTTP and HTTPS traffic to access the web interface later.
   - **External IP**: Choose **Static IP** for easier access to your EVE-NG instance.

4. Click **Create** to launch the instance.

## Step 4: SSH Into the VM
1. Once the VM is up, click the **SSH** button next to your instance to open a terminal window.
2. Update the system packages:
   ```bash
   sudo apt update
   sudo apt upgrade -y
