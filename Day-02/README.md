# AWS IAM (Identity and Access Management)

**AWS IAM (Identity and Access Management)** is a security service from Amazon Web Services that allows you to control access to your AWS resources. IAM helps you manage users, groups, roles, and permissions for secure and organized access.

## Key Features:

- **Manage Users, Groups, and Roles**: Control access to AWS resources by creating users, organizing them into groups, and assigning roles for temporary access.
- **Granular Permissions with Policies**: Define what actions are allowed or denied on AWS resources using policies written in JSON format.
- **Security Best Practices**: Implements the principle of least privilege, ensuring that users and entities have only the necessary permissions for their tasks. It also supports **Multi-Factor Authentication (MFA)** for enhanced security and provides an **audit trail** to track changes and activity.
- **Temporary Access**: Use roles for granting temporary access to AWS resources for external entities or services, such as applications or cross-account access.

## Components of IAM:

- **Users**: Individual entities (people, services, applications) that interact with AWS resources. Users have unique credentials (password or access keys) for authentication.
- **Groups**: Collections of users with similar access needs. Permissions are assigned to groups to simplify access management.
- **Roles**: Temporary access grants, typically used by applications or services needing access to AWS resources on behalf of users or other services.
- **Policies**: JSON documents that define the permissions and actions allowed on AWS resources. Policies can be attached to users, groups, or roles to specify allowed/denied actions.

## Security and Compliance:

- **Principle of Least Privilege**: IAM ensures that users are granted only the permissions they need to perform their jobs, minimizing security risks.
- **Audit Trail**: IAM provides logs for tracking user activities and permissions changes, ensuring accountability and compliance.

**IAM is an essential part of AWS security**—enabling you to secure and manage access to AWS resources efficiently and effectively.

---

## - How IAM works

![How IAM works](https://github.com/user-attachments/assets/d883bf2c-7a24-48e4-8da2-b88c848863a2)

--- 

## - Comapre IAM identities and credentials 

![IAM Terms - Resource, Entity, Identity](https://github.com/user-attachments/assets/ff66f8ba-3f98-4464-ba70-cef992def1eb)


![IAM Identity Center](https://github.com/user-attachments/assets/c404dc36-409a-424e-84a1-cef5610a6657)

---

### [AWS Documentation on Identity and Access Management (IAM) - User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)

---
