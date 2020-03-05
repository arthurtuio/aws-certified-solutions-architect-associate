# IAM

IAM permite gerenciar o uso e o nível de acesso ao console da AWS, oferecendo:
- Centralised control of your AWS account.
- Shared Accesss to your AWS account.
- Granular Permissions.
- Identity Federation (Active Directory, Facebook, LinkedIn, etc). - SAML -> Google
- Multifactor Authentication (Username - Password - Special Code).
- Provide temporary Access for user/devices and services where necessay
- Allows you to set up your own password rotation policy.
- Integrates with many different AWS services.
- Supports PCI DSS compliance (Credit Card complience).

## IAM Terminology

- Users: end user (people, organization, etc.).
- Groups: Collection of users. Each user in the group will inherit the permissions of the group.
- Roles: you crea tols and then assing then to AWS Resources.
- Policies: made up of Policy documents. These documents are in a format called JSON and they give permissions as to that User / Group / Role is able to do.

## Resumo

Com IAM podemos criar grupos de permissões, ex:
Seu time de marketing precisa de acesso ao s3 bucket para ler e gravar imagens, faz sentido então criar um grupo chamado,
por exemplo, mkt_bucket_upload e criar uma policy com as permissões de upload de arquivos para o bucket. 
Desta maneira basta vincular a policy ao grupo e adicionar os usuários de marketing ao grupo mkt_bucket_upload.
Exemplo real: Nosso grupo Developers.
Também temos Papéis (Roles) são permissões para os serviços da AWS, como EC2, Lambda...
Também temos Policies, as policies podem ser anexadas a um grupo ou a uma Role de usuário, ali será definido as permissões e atribuídas ao grupo ou a Role.

## IAM Lab

### Security Status
- Delete your root Access keys
- Activate MFA on your root account
- Create a individual IAM user
- Use groups to assign permissions
- Apply an IAM password policy


### Create a IAM user

1. User Details: User Name, Access type: Programmatic access (access key ID and secret access key and/or AWS Management Console access (password to sing-in)
2. Set Permission: Add to a Group, Copy permissions from existing user or attach existing policies directly
3. After creation set all the secrets of the user

### Create a Group

- Group name & Attach the Policies

### Polices

Admin:
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "*",
      "Resource": "*"
    }
  ]
}
```

### Create Roles
Like a user


## Exam Tips
- IAM is universal, it does not apply to regions at this time.
- Root account - Gog Mode - first account created
- New Users have NO PERMISSIONS & are assigned to Access Key ID-Secret Access Key then first created
- Access Key ID-Secret Access Key (CLI) != Password (Console)
- Access Key ID-Secret Access Key are seen only once (in the creation or you have to regenerated).
- Always setup MAF on your root account.
- You can create and customise your own password rotation policies

## Identity Access Management Quiz (Architect)

1. What is an additional way to secure the AWS accounts of both the root account and new users alike?
- [ ] Implement Multi-Factor Authentication for all accounts.
- [ ] Store the access key id and secret access key of all users in a publicly accessible plain text document on S3 of which only you and members of your organization know the address.
- [ ] Configure the AWS Console so that you can only log in to it from a specific IP Address range
- [ ] Configure the AWS Console so that you can only log in to it from your internal network IP address range.

2. Which of the following is not a component of IAM?
- [ ] Roles
- [ ] Users
- [ ] Groups
- [ ] Organizational Units

3. When you create a new user, that user ________.
- [ ] Will be able to log in to the console anywhere in the world, using their access key ID and secret access key
- [ ] Will be able to interact with AWS using their access key ID and secret access key using the API, CLI, or the AWS SDKs
- [ ] Will only be able to log in to the console in the region in which that user was created
- [ ] Will be able to log in to the console only after multi-factor authentication is enabled on their account

4. Which statement best describes IAM?
- [ ] IAM allows you to manage users, groups, roles, and their corresponding level of access to the AWS Platform.
- [ ] IAM allows you to manage users' passwords only. AWS staff must create new users for your organization. This is done by raising a ticket.
- [ ] IAM allows you to manage permissions for AWS resources only.
- [ ] IAM stands for Improvised Application Management, and it allows you to deploy and manage applications in the AWS Cloud.

5. Using SAML (Security Assertion Markup Language 2.0), you can give your federated users single sign-on (SSO) access to the AWS Management Console.
- [ ] True
- [ ] False

6. In what language are policy documents written?
- [ ] Python
- [ ] JSON
- [ ] Node.js
- [ ] Java

7. What is the default level of access a newly created IAM User is granted?
- [ ] Read-only access to all AWS services.
- [ ] No access to any AWS services
- [ ] Administrator access to all AWS services
- [ ] Power user access to all AWS services

8. Which of the following is not a feature of IAM?
- [ ] IAM offers centralized control of your AWS account
- [ ] IAM integrates with existing active directory account allowing single sing-on
- [ ] IAM offers fine-grained access control to AWS resources
- [ ] IAM allows you to set up biometric authentication, so that no password are required

9. You have a client who is considering a move to AWS. In establishing a new account, what is the first thing the company should do?
- [ ] Set up an account using Cloud Search
- [ ] Set up an account using their company email address
- [ ] Set up an account via SQS (Simple Queue Service)
- [ ] Set up an account via SNS (Simple Notification Service)

10. A new employee has just started work, and it is your job to give her administrator access to the AWS console. You have given her a user name, an access key ID, a secret access key, and you have generated a password for her. She is now able to log in to the AWS console, but she is unable to interact with any AWS services. What should you do next?
- [ ] Grant her Administrator access by adding her to the Administrators' group.
- [ ] Require multi-factor authentication for her user account.
- [ ] Ensure she is logging in to the AWS console from your corporate network and not the normal internet.
- [ ] Tell her to log out and try logging back in again.

11. What level of access does the "root" account have?
- [ ] Read-only access
- [ ] Power User Access
- [ ] Administrator Access
- [ ] No access

12. Power User Access allows ________.
- [ ] Full Access to all AWS services and resources.
- [ ] Read-only access to all AWS services and resources.
- [ ] Read-only access to all AWS services and resources.
- [ ] Users to inspect the source code of the AWS platform.

13. You are a solutions architect working for a large engineering company that are moving from a legacy infrastructure to AWS. You have configured the company's first AWS account and you have set up IAM. Your company is based in Andorra, but there will be a small subsidiary operating out of South Korea, so that office will need its own AWS environment. Which of the following statements is true?
- [ ] You will then need to configure Users and Policy Documents for each region, respectively.
- [ ] You will need to configure Users and Policy Documents only once, as these are applied globally.
- [ ] You will need to configure your users regionally, however your policy documents are global.
- [ ] You will need to configure your policy documents regionally, however your users are global.

14. You are a security administrator working for a hotel chain. You have a new member of staff who has started as a systems administrator, and she will need full access to the AWS console. You have created the user account and generated the access key id and the secret access key. You have moved this user into the group where the other administrators are, and you have provided the new user with their secret access key and their access key id. However, when she tries to log in to the AWS console, she cannot. Why might that be?
- [ ] You have not applied the "log in from console" policy document to the user. You must apply this first so that they can log in.
- [ ] Your user is trying to log in from the AWS console from outside the corporate network. This is not possible.
- [ ] You have not yet activated multi-factor authentication for the user, so by default they will not be able to log in.
- [ ] You cannot log in to the AWS console using the Access Key ID / Secret Access Key pair. Instead, you must generate a password for the user, and supply the user with this password and your organization's unique AWS console login URL.

15. Every user you create in the IAM systems starts with ________.
- [ ] Full Permissions
- [ ] Partial Permissions
- [ ] No Permissions

16. A __________ is a document that provides a formal statement of one or more permissions.
- [ ] Policy
- [ ] User
- [ ] Group
- [ ] Role

17. You have created a new AWS account for your company, and you have also configured multi-factor authentication on the root account. You are about to create your new users. What strategy should you consider in order to ensure that there is good security on this account.
- [ ] Enact a strong password policy: user passwords must be changed every 45 days, with each password containing a combination of capital letters, lower case letters, numbers, and special symbols.
- [ ] Require users only to be able to log in using biometric authentication.
- [ ] Restrict login to the corporate network only.
- [ ] Give all users the same password so that if they forget their password they can just ask their co-workers.

<details><summary>RESPOSTAS</summary>
<p>1. What is an additional way to secure the AWS accounts of both the root account and new users alike?</p>
<p>Implement Multi-Factor Authentication for all accounts.</p>
<p>2. Which of the following is not a component of IAM?</p>
</p>Organizational Units</p>
</p>3. When you create a new user, that user ________.</p>
</p>Will be able to interact with AWS using their access key ID and secret access key using the API, CLI, or the AWS SDKs</p>
</p>4. Which statement best describes IAM?</p>
- [ ] IAM allows you to manage users, groups, roles, and their corresponding level of access to the AWS Platform.
- [ ] IAM allows you to manage users' passwords only. AWS staff must create new users for your organization. This is done by raising a ticket.
- [ ] IAM allows you to manage permissions for AWS resources only.
- [ ] IAM stands for Improvised Application Management, and it allows you to deploy and manage applications in the AWS Cloud.
</p>5. Using SAML (Security Assertion Markup Language 2.0), you can give your federated users single sign-on (SSO) access to the AWS Management Console.
- [ ] True
- [ ] False
</p>6. In what language are policy documents written?</p>
</p>JSON</p>
</p>7. What is the default level of access a newly created IAM User is granted?</p>
</p>No access to any AWS services</p>
</p>8. Which of the following is not a feature of IAM?</p>
</p>IAM allows you to set up biometric authentication, so that no password are required</p>
</p>9. You have a client who is considering a move to AWS. In establishing a new account, what is the first thing the company should do?
</p>Set up an account using their company email address</p>
</p>10. A new employee has just started work, and it is your job to give her administrator access to the AWS console. You have given her a user name, an access key ID, a secret access key, and you have generated a password for her. She is now able to log in to the AWS console, but she is unable to interact with any AWS services. What should you do next?
</p>Grant her Administrator access by adding her to the Administrators' group.</p>

11. What level of access does the "root" account have?
- [ ] Read-only access
- [ ] Power User Access
- [ ] Administrator Access
- [ ] No access

12. Power User Access allows ________.
- [ ] Full Access to all AWS services and resources.
- [ ] Read-only access to all AWS services and resources.
- [ ] Read-only access to all AWS services and resources.
- [ ] Users to inspect the source code of the AWS platform.

13. You are a solutions architect working for a large engineering company that are moving from a legacy infrastructure to AWS. You have configured the company's first AWS account and you have set up IAM. Your company is based in Andorra, but there will be a small subsidiary operating out of South Korea, so that office will need its own AWS environment. Which of the following statements is true?
- [ ] You will then need to configure Users and Policy Documents for each region, respectively.
- [ ] You will need to configure Users and Policy Documents only once, as these are applied globally.
- [ ] You will need to configure your users regionally, however your policy documents are global.
- [ ] You will need to configure your policy documents regionally, however your users are global.

14. You are a security administrator working for a hotel chain. You have a new member of staff who has started as a systems administrator, and she will need full access to the AWS console. You have created the user account and generated the access key id and the secret access key. You have moved this user into the group where the other administrators are, and you have provided the new user with their secret access key and their access key id. However, when she tries to log in to the AWS console, she cannot. Why might that be?
- [ ] You have not applied the "log in from console" policy document to the user. You must apply this first so that they can log in.
- [ ] Your user is trying to log in from the AWS console from outside the corporate network. This is not possible.
- [ ] You have not yet activated multi-factor authentication for the user, so by default they will not be able to log in.
- [ ] You cannot log in to the AWS console using the Access Key ID / Secret Access Key pair. Instead, you must generate a password for the user, and supply the user with this password and your organization's unique AWS console login URL.

15. Every user you create in the IAM systems starts with ________.
- [ ] Full Permissions
- [ ] Partial Permissions
- [ ] No Permissions

16. A __________ is a document that provides a formal statement of one or more permissions.
- [ ] Policy
- [ ] User
- [ ] Group
- [ ] Role

17. You have created a new AWS account for your company, and you have also configured multi-factor authentication on the root account. You are about to create your new users. What strategy should you consider in order to ensure that there is good security on this account.
- [ ] Enact a strong password policy: user passwords must be changed every 45 days, with each password containing a combination of capital letters, lower case letters, numbers, and special symbols.
- [ ] Require users only to be able to log in using biometric authentication.
- [ ] Restrict login to the corporate network only.
- [ ] Give all users the same password so that if they forget their password they can just ask their co-workers.</p>
</details>

## Identity Access Management Quiz (Developer)

1. Qual das opções a seguir NÃO é um recurso do IAM?

- [ ] Controle centralizado da sua conta da AWS
- [ ] Integra-se à sua conta do Active Directory existente, permitindo single sign-on
- [ ] Controle de acesso refinado aos recursos da AWS
- [ ] Permite configurar a autenticação biométrica, de modo que nenhuma senha seja necessária.

2. A AWS recomenda que as instâncias do EC2 tenham credenciais armazenadas nelas, para que elas possam acessar outros recursos (como buckets S3).

- [ ] Verdadeiro
- [ ] Falso.

3. Qual entidade do IAM você pode usar para delegar o acesso aos seus recursos da AWS a usuários, grupos ou serviços?

- [ ] IAM User
- [ ] IAM Web Identity Federation
- [ ] IAM Role.
- [ ] IAM Group.

4. O que é uma policy do IAM?

- [ ] Um arquivo CSV que contém a Accesss Key e Secret Accesss Key do usuário
- [ ] Um documento JSON que define uma ou mais permissões.
- [ ] Um arquivo que contém a chave SSH privada de um usuário.
- [ ] A policy que determina como sua conta da AWS será paga.

5. Qual é a melhor maneira de permitir que sua instância do EC2 leia arquivos em um bucket S3?

- [ ] Crie um novo usuário do IAM e conceda acesso de leitura ao S3. Armazene as credenciais do usuário localmente na instância do EC2 e configure seu aplicativo para fornecer as credenciais a cada solicitação de API
- [ ] Configure uma política de bucket que conceda acesso de leitura com base no nome da instância do EC2
- [ ] Crie uma role do IAM com acesso de leitura ao S3 e atribua a role à instância do EC2.
- [ ] Crie uma nova role do IAM e conceda acesso de leitura ao S3. Armazene as credenciais da role localmente na instância do EC2 e configure seu aplicativo para fornecer as credenciais a cada solicitação da API.

6. Qual afirmação melhor descreve o IAM?

- [ ] O IAM permite gerenciar usuários, grupos e roles e o nível correspondente de acesso à plataforma da AWS.
- [ ] O IAM permite gerenciar apenas as senhas dos usuários. A equipe da AWS deve criar novos usuários para sua organização. Isso é feito levantando um ticket.
- [ ] O IAM permite gerenciar permissões apenas para recursos da AWS.
- [ ] O IAM significa Gerenciamento Improvisado de Aplicativos e permite implantar e gerenciar aplicativos na nuvem da AWS.

<details><summary>RESPOSTAS</summary>
<p>1. Qual das opções a seguir NÃO é um recurso do IAM?</p>
<p>Permite configurar a autenticação biométrica, de modo que nenhuma senha seja necessária.</p>
<p>2. A AWS recomenda que as instâncias do EC2 tenham credenciais armazenadas nelas, para que elas possam acessar outros recursos (como buckets S3).</p>
<p>Falso</p>
<p>3. Qual entidade do IAM você pode usar para delegar o acesso aos seus recursos da AWS a usuários, grupos ou serviços?</p>
<p>IAM Role.</p>
<p>4. O que é uma policy do IAM?</p>
<p>Um documento JSON que define uma ou mais permissões.</p>
<p>5. Qual é a melhor maneira de permitir que sua instância do EC2 leia arquivos em um bucket S3?</p>
<p>Crie uma role do IAM com acesso de leitura ao S3 e atribua a role à instância do EC2.</p>
<p>6. Qual afirmação melhor descreve o IAM?</p>
<p>O IAM permite gerenciar usuários, grupos e roles e o nível correspondente de acesso à plataforma da AWS.</p>
</details>
