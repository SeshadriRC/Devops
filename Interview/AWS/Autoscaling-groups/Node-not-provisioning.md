<img width="1875" height="883" alt="image" src="https://github.com/user-attachments/assets/91fe12e9-9a20-4a87-83ae-8b534b1fe276" />

Exactly — you’ve already highlighted the main causes. Let’s put it in a **clear, interview-friendly explanation**:

---

## **Issue: ASG with 100% Spot Instances unable to provision new nodes**

### **Possible Causes:**

1. **Spot Price Increase / Spot Capacity Unavailability**

   * Spot instances are **spare AWS capacity** and their availability depends on demand.
   * If the **current spot price exceeds your bid**, or **AWS doesn’t have available capacity**, new instances **cannot be launched**.
   * This is a common reason why an ASG fails to scale up.

2. **EC2 Quota Limits**

   * AWS imposes **service quotas (limits)** on the number of EC2 instances per region per instance type.
   * If the ASG tries to provision instances beyond this quota, scaling will **fail**.
   * Example: You have a quota of 20 `t3.large` instances, but the ASG tries to launch 25.

---

### **Why 100% Spot Instances is Risky**

* Spot instances can be **terminated anytime** by AWS when capacity is needed.
* If your ASG relies only on spot instances and there’s no fallback (like **On-Demand instances**), **your application may face downtime**.

---

### **Best Practices**

1. **Mixed Instances Policy**: Use a combination of Spot + On-Demand to improve reliability.
2. **Spot Allocation Strategy**: Diversify across instance types and AZs.
3. **Monitor Quotas**: Ensure EC2 limits are sufficient for scaling needs.
4. **Set up notifications** for Spot instance interruptions.

---

✅ **Interview Answer Template:**

> “If an Auto Scaling Group using only Spot instances is unable to provision new nodes, it’s usually due to either a **rise in spot prices / lack of spot capacity** or hitting the **EC2 quota limits** in that region.
> 100% Spot ASGs are risky for critical workloads because spot instances can be terminated anytime. It’s recommended to use a **mixed instances policy** combining spot and on-demand instances for reliability.”

---

If you want, I can also **draw a small diagram showing ASG with spot + on-demand for scaling reliability**, which is great for interviews.

Do you want me to do that?
