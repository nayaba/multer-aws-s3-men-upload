### How to Sign Up for the AWS Free Tier

Here’s a step-by-step breakdown of the AWS setup process with specific guidance to help you avoid unnecessary costs:

---

### **1. Sign Up for AWS**
- **Go to the AWS Website**: [AWS Sign-Up Page](https://aws.amazon.com/)
- **Follow the Steps**:
  - **Contact Info**: Use accurate information.
  - **Billing Info**: A credit/debit card is required, but AWS offers a **Free Tier**. Ensure you stay within the Free Tier limits.
  - **Confirm Identity**: Enter the OTP sent to your phone.
  - **Support Plan**: Select the **Basic Plan** (free).

---

### **2. Activate Free Tier Alerts**
- **Enable Free Tier Billing Alerts**:
  - Log in to the AWS Management Console.
  - Go to **Billing Dashboard** > **Budgets**.
  - Set up a **Free Tier Usage Alert**:
    - Specify an alert threshold (e.g., 80% of Free Tier usage).
    - Add your email for notifications.

---

### **3. Set Up a Budget**
- In **Billing Dashboard** > **Budgets**, create a budget for monthly costs:
  - Set the budget to $0.01 (or your comfort level).
  - Enable email alerts when costs exceed your threshold.
  
---

### **4. Configure Cost Anomaly Detection**
- In **Billing Dashboard**, set up **Cost Anomaly Detection**:
  - Add notifications for unusual charges beyond Free Tier usage.

---

### **5. EC2 Instance Setup**
1. **Go to EC2 Dashboard**:
   - Open the AWS Management Console, search for "EC2."
   - Click **Launch Instance**.
2. **Free Tier Eligibility**:
   - Ensure you select an instance type marked as **Free Tier Eligible** (e.g., **t2.micro**).
3. **Setup Steps**:
   - Choose **Ubuntu** as the OS.
   - Create a key pair for SSH access.
   - Add a **user data script** (use provided script for dependencies).
   - Launch the instance.

**Important**: AWS Free Tier allows 750 hours of EC2 usage per month for the first 12 months. Monitor usage to avoid charges.

---

### **6. Security Settings**
1. Open only necessary ports (e.g., 3000 for your app).
2. Always choose **"Anywhere-IPv4"** carefully to avoid exposing your app unnecessarily.
3. Use strong passwords and secure SSH connections.

---

### **7. App Deployment**
1. Deploy apps on a single instance (if possible) to avoid multiple instance charges.
2. Use ports efficiently:
   - Assign different ports (e.g., 3000, 4000) for multiple apps instead of new instances.

---

### **8. Monitoring Your Instance**
1. Use **Down Notifier** to monitor your app's uptime.
2. Check **AWS EC2 Dashboard** frequently to ensure the instance is running correctly.

---

### **9. Avoid Charges**
- **Stop Instance When Not in Use**:
  - Stopping the instance prevents additional costs. (Ensure you have saved your work before stopping.)
- **Monitor Billing**:
  - Go to **Billing Dashboard** and review costs frequently.

---

### **10. Deletion**
If you're done using AWS, delete the instance to ensure there are no surprise charges:
1. Terminate the instance in the EC2 Dashboard.
2. Remove any unused resources like S3 buckets or Elastic IPs.

---

Following these steps ensures you can use AWS effectively while staying within the Free Tier limits and avoiding unexpected charges. If you notice unusual charges, contact AWS Support immediately to resolve them.
