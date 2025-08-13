It is vital to administrate and correctly define the various levels of access to an information technology system individuals require. 
The levels of access given to individuals are determined on two primary factors:
- The individual's role/function within the organisation
- The sensitivity of the information being stored on the system
Two key concepts are used to assign and manage the access rights of individuals: Privileged Identity Management (**PIM**) and Privileged Access Management (or **PAM** for short).
Initially, these two concepts can seem to overlap; however, they are different from one another. PIM is used to translate a user's role within an organisation into an access role on a system. Whereas PAM is the management of the privileges a system's access role has, amongst other things.
What is essential when discussing privilege and access controls is the principle of least privilege. Simply, users should be given the minimum amount of privileges, and only those that are absolutely necessary for them to perform their duties. Other people should be able to trust what people write to.
As we previously mentioned, PAM incorporates more than assigning access. It also encompasses enforcing security policies such as password management, auditing policies and reducing the attack surface a system faces.

---
According to a security model, any system or piece of technology storing information is called an information system, which is how we will reference systems and devices in this task.
Let's explore some popular and effective security models used to achieve the three elements of the CIA triad.
## **The Bell-La Padula Model**
The Bell-La Padula Model is used to achieve confidentiality. This model has a few assumptions, such as an organisation's hierarchical structure it is used in, where everyone's responsibilities/roles are well-defined.
The model works by granting access to pieces of data (called objects) on a strictly need to know basis. This model uses the rule "no write down, no read up".
![[0e6e5d9d80785fc287b4a67e1453b295.png]]
![[Pasted image 20250729121711.png]]
The Bell LaPadula Model is popular within organisations such as governmental and military. This is because members of the organisations are presumed to have already gone through a process called vetting. Vetting is a screening process where applicant's backgrounds are examined to establish the risk they pose to the organisation. Therefore, applicants who are successfully vetted are assumed to be trustworthy - which is where this model fits in.

## **Biba Model**
The Biba model is arguably the equivalent of the Bell-La Padula model but for the integrity of the CIA triad.
This model applies the rule to objects (data) and subjects (users) that can be summarised as "no write up, no read down". This rule means that subjects **can** create or write content to objects at or below their level but **can only** read the contents of objects above the subject's level.
Let's compare some advantages and disadvantages of this model in the table below:
![[Pasted image 20250729122315.png]]
![[895ba351ef24ef6495d290222e49470e.png]]
The Biba model is used in organisations or situations where integrity is more important than confidentiality. For example, in software development, developers may only have access to the code that is necessary for their job. They may not need access to critical pieces of information such as databases, etc.
