---
title: Developer Info
subtitle: Microsoft Office 365
book: integrations
weight: 10
chapter: office365
layout: chapter
---
Microsoft Office 365 allows for external applications to integrate through the use of its Azure platform. Currently **&lt;<span class="text-primary">form</span>.<span class="text-secondary">io</span>&gt;** provides a mechanism where it can act on behalf of your account through its Server-to-Server authentication mechanism. To get your project working with Microsoft Office 365, please walk through the following steps:

  - ### Step 1: [Create your Office 365 Business Account](https://products.office.com/en-us/business/compare-office-365-for-business-plans){:target="_blank"}
  - ### Step 2: [Associate your Office 365 account with Azure](https://msdn.microsoft.com/office/office365/HowTo/setup-development-environment#bk_CreateAzureSubscription){:target="_blank"}
  - ### Step 3: Register your application with Azure Active Directory
    - Sign into the [Azure Portal](https://portal.azure.com/){:target="_blank"} using your Office 365 Developer Site credentials.

    - Click Azure Active Directory on the left menu, then click on the ***App registrations***, and then click on the ***New registration***.

      ![](/assets/img/office365/v2/azure-new-app.png){: .img-fluid .img-thumbnail }

    - Fill out your application information.

    - Provide an application URL. It can be **localhost** if you are just using it for **&lt;<span class="text-primary">form</span>.<span class="text-secondary">io</span>&gt;** integration.

    - Click on the ***Register*** button.

      ![](/assets/img/office365/v2/azure-register-app.png){: .img-fluid .img-thumbnail }

    - Create a new API permistion. Click on the ***Expose an API***, then click on the ***Add a scope*** and fill scope information, for example "Contacts.ReadWrite".

    - Click on the ***Add scope*** button.

      ![](/assets/img/office365/v2/azure-add-contacts-scope.png){: .img-fluid .img-thumbnail }

    - Click on ***Api permissions***, then click on the ***Add a permission***, then click on the ***My APIs*** and choose our created persmission.

      ![](/assets/img/office365/v2/azure-add-api-permission.png){: .img-fluid .img-thumbnail }

    - Check persmission to enable it.

    - Click on the ***Add permissions*** button.

      ![](/assets/img/office365/v2/azure-check-permission.png){: .img-fluid .img-thumbnail }

    - The next thing you need to do is generate the **keyCredentials** and place that in the manifest file. The easiest way to do this is to utilize a Node.js utility called **keycred**.

    - Before you continue, you will need to make sure you install Node.js by following the installation instructions @ [https://nodejs.org](https://nodejs.org).

    - Now that you have Node.js installed, you can install **keycred** library by typing the following in your terminal.

      ```
      npm install -g keycred
      ```

    - This installs a Command Line Interface (CLI) tool for generating key credentials for your application. You can run the utility by simply typing the following in your terminal.

      ```
      keycred
      ```

    - This utility will allow you to either pick an existing certificate, or generate a new one. It is probably easiest to let it generate a new one for you. Here is what the output look like.

      ```
      ○ → keycred
      prompt: Would you like to generate a new Certificate or use an existing one?

      1.) Generate New
      2.) Use Existing:  1
      prompt: Country Name (2 letter code) [AU]:  US
      prompt: State or Province Name (full name) [Some-State]:  Texas
      prompt: Locality Name (eg, city) []:  Dallas
      prompt: Organization Name (eg, company) [Internet Widgits Pty Ltd]:  Form.io
      prompt: Organizational Unit Name (eg, section) []:  IT
      prompt: Common Name (e.g. server FQDN or YOUR name):  Travis Tidwell <travis@form.io>
      Generating key pairs
      Creating a certificate.
      Signing the certificate.

      Key Credentials:
      {
      "customKeyIdentifier": "Hm69r8sj1uhRKiKEcZ1v3yIUqxk=",
      "value": "MIID9zCCAt+gAwIBAgIBATANBgkqhkiG9w0BAQUFADB3MSgwJgYDVQQDEx9UcmF2aXMgVGlkd2VsbCA8dHJhdmlzQGZvcm0uaW8+MQswCQYDVQQGEwJVUzEOMAwGA1UECBMFVGV4YXMxDzANBgNVBAcTBkRhbGxhczEQMA4GA1UEChMHRm9ybS5pbzELMAkGA1UECxMCSVQwHhcNMTUwODEwMDMwMzQyWhcNMTYwODEwMDMwMzQyWjB3MSgwJgYDVQQDEx9UcmF2aXMgVGlkd2VsbCA8dHJhdmlzQGZvcm0uaW8+MQswCQYDVQQGEwJVUzEOMAwGA1UECBMFVGV4YXMxDzANBgNVBAcTBkRhbGxhczEQMA4GA1UEChMHRm9ybS5pbzELMAkGA1UECxMCSVQwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQC18uROe+X0/26zcgJTdluVd7tK4T407hGXTccNCebuKP2qUsh+/i0imLHLYZ3xs5hbSPPvjRF/wVm3CowZbnaCNNnd2WaUHFWBr+4ltTtxlpByPJIQ6rIMPvHkhfDUV05MN8ZvXUTZQEceVF/Y5dP/muB6v8zMeCRcEbvXNKHmDu8Ss6O5CdfXjoSwB4721NxmegHATdlet8JzaVal6/c4L5vzofVcu/wnF4f8bS2J4D5SBSh2WhpvMcjPsrrOmRfUIfR8rr7k4ZgBotJHsPwaCe/z10Q7K2Q3FpUpzgZ7t86gCVaDaEbR8aUMpEP2/Xhgk+T0nQIb1/Vh0dRIvDWJAgMBAAGjgY0wgYowDAYDVR0TBAUwAwEB/zALBgNVHQ8EBAMCAvQwOwYDVR0lBDQwMgYIKwYBBQUHAwEGCCsGAQUFBwMCBggrBgEFBQcDAwYIKwYBBQUHAwQGCCsGAQUFBwMIMBEGCWCGSAGG+EIBAQQEAwIA9zAdBgNVHQ4EFgQUnUysxhgEYmbnewKQOmWPcd1s7ncwDQYJKoZIhvcNAQEFBQADggEBAEKIfTm5SRDGM2KJ40cGGfnpKDgzJ8a+dXjqKMeooh/U9qWUVTmjXyRe8PdshB0ZkPXWWj+iybpT2YlZ6oFo4KCU6/6fxPrg142Y72XE5ZgnZ29Ij86Mh67QWmgIhB0LonYfCjJrLjhgvx+ajGlCcxmwcmXJVSZOkyFCwBChilrdAsayLokOfjbUmE+we6Og7bbDmUIbzE4lsuNqJFR0pUZV54UpaV+d6mNfjR/1r4b0B2WRxO0TmVinzNjiT6rpjrxbYeJKaEQ0wxnzX++dY2drUffqQGs4/aJn/CCF0QsiS7IdHtPVBZ058LPExfYqQfsJF6m6DGCJUA7pNjOSZJ8=",
      "keyId": "ea8ec579-fcbe-4ce5-ba65-2f5246dfdd6c",
      "usage": "Verify",
      "type": "AsymmetricX509Cert"
      }

      Private Key:
      -----BEGIN RSA PRIVATE KEY-----
      MIIEpAIBAAKCAQEAtfLkTnvl9P9us3ICU3ZblXe7SuE+NO4Rl03HDQnm7ij9qlLI
      fv4tIpixy2Gd8bOYW0jz740Rf8FZtwqMGW52gjTZ3dlmlBxVga/uJbU7cZaQcjyS
      EOqyDD7x5IXw1FdOTDfGb11E2UBHHlRf2OXT/5rger/MzHgkXBG71zSh5g7vErOj
      uQnX146EsAeO9tTcZnoBwE3ZXrfCc2lWpev3OC+b86H1XLv8JxeH/G0tieA+UgUo
      dloabzHIz7K6zpkX1CH0fK6+5OGYAaLSR7D8Ggnv89dEOytkNxaVKc4Ge7fOoAlW
      g2hG0fGlDKRD9v14YJPk9J0CG9f1YdHUSLw1iQIDAQABAoIBADzx3wdq+NvXs3zn
      81+BhavLLzElwXB5TesgYkw7xN6BXHZwxDfFa9jqzKMTT5RmU+I/zXWwCuyAF0z4
      e3UJSyjSCygEaheyZfHKvDplOkQR6tBY+ZQxCPKRIvUo6EI3/EILbKxg0W7z2N6P
      5IsCcMBtYEO9exwCIiu0xPaQ6qnkRFYTUJWRs1N310Q437/IAf+Ajon+lxqoMgif
      7qlu8G31LNj8waP68VF2Oyj5tzy1xgrbiMgNEEx5rMrfgSfxI8KRFD5j9qXddcr/
      0bMIsjX6nM8GEIQgIeKnv9LwPfdTpoaas4njV5EAHMM+UZwqiHpzNZQwI5EIVTC4
      Gr/X0wECgYEA/FK2l/i3cefstunBDxIO6wVHH6Ywus3eC853MWrjHoVdYyuwYTFn
      AyKDCs/chbUzU5bNKPScuQzNtxeHSF4xSSBBdvErvIhQsqTLCZDBxiEY5jjVJ4nM
      HwtphXKkt6ZEtCdwe+v7w3R9NxsziAiG3WJHpr6dCt31sYHJNAFn9gcCgYEAuJmm
      BI6pnjnDo0c06fY43GTml1fofQ7+Iwr8si6/Zupb/Ck7KE/xdYO+4lCFFagdlaOw
      rb0xhyaQ7ddfjTiWsP3lcupjykUxyUaSyN4u9owyf4rVtZOyazPGCQ9qykRzgGpm
      6pWqEg4QoWcMerCPjd+/XjfMkDsiAYfPBeB9E+8CgYEAsiHGrHU3NOAR+nP+CqCq
      DXtVYd+WyVprJxSkiyu1mad3bXq7c3JIEit8LdbfDToGOT3eKsq+FdoXJOokZI+y
      5bgy30CVquzlY6j5ehBK7JATHv0CZn5er5AD9+UeqlRkbnblb2cC/1Kuz4eRkrjK
      VWJ7yRkKj1BxktZYcDGJ7mMCgYBGjEHMQBFGrOC9h7sLJtQ4Nj+B21C4aoBpOAu2
      tPLlC++3gLJhB5xJwt2yc/9IdYVI6NPesg05j12X59VWjSfZ4E5OCpG0fZk3SMdU
      CMV957psz8w4podrPNAUKlYvktVpYECQqj3ixIlKha1ZPhy+paHnCgAdptuqJhLS
      NibW9QKBgQDiWsUq4AusD3TqQ1lfbrlK8/eFXcN9ropZEy5NtMX6+s6R3FVN/pMJ
      UAH4YDa37GYp6eq6WClbQEXjXcXC87Kiffv+xA0Up3mjN5ebAoesAtzcqPnhrqE+
      oNuARIfZMcMdv1zX+iPA+jjg6FJmQwOmz3abGpxMaNcYj9ChNcIf8Q==
      -----END RSA PRIVATE KEY-----


      Certificate:
      -----BEGIN CERTIFICATE-----
      MIID9zCCAt+gAwIBAgIBATANBgkqhkiG9w0BAQUFADB3MSgwJgYDVQQDEx9UcmF2
      aXMgVGlkd2VsbCA8dHJhdmlzQGZvcm0uaW8+MQswCQYDVQQGEwJVUzEOMAwGA1UE
      CBMFVGV4YXMxDzANBgNVBAcTBkRhbGxhczEQMA4GA1UEChMHRm9ybS5pbzELMAkG
      A1UECxMCSVQwHhcNMTUwODEwMDMwMzQyWhcNMTYwODEwMDMwMzQyWjB3MSgwJgYD
      VQQDEx9UcmF2aXMgVGlkd2VsbCA8dHJhdmlzQGZvcm0uaW8+MQswCQYDVQQGEwJV
      UzEOMAwGA1UECBMFVGV4YXMxDzANBgNVBAcTBkRhbGxhczEQMA4GA1UEChMHRm9y
      bS5pbzELMAkGA1UECxMCSVQwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIB
      AQC18uROe+X0/26zcgJTdluVd7tK4T407hGXTccNCebuKP2qUsh+/i0imLHLYZ3x
      s5hbSPPvjRF/wVm3CowZbnaCNNnd2WaUHFWBr+4ltTtxlpByPJIQ6rIMPvHkhfDU
      V05MN8ZvXUTZQEceVF/Y5dP/muB6v8zMeCRcEbvXNKHmDu8Ss6O5CdfXjoSwB472
      1NxmegHATdlet8JzaVal6/c4L5vzofVcu/wnF4f8bS2J4D5SBSh2WhpvMcjPsrrO
      mRfUIfR8rr7k4ZgBotJHsPwaCe/z10Q7K2Q3FpUpzgZ7t86gCVaDaEbR8aUMpEP2
      /Xhgk+T0nQIb1/Vh0dRIvDWJAgMBAAGjgY0wgYowDAYDVR0TBAUwAwEB/zALBgNV
      HQ8EBAMCAvQwOwYDVR0lBDQwMgYIKwYBBQUHAwEGCCsGAQUFBwMCBggrBgEFBQcD
      AwYIKwYBBQUHAwQGCCsGAQUFBwMIMBEGCWCGSAGG+EIBAQQEAwIA9zAdBgNVHQ4E
      FgQUnUysxhgEYmbnewKQOmWPcd1s7ncwDQYJKoZIhvcNAQEFBQADggEBAEKIfTm5
      SRDGM2KJ40cGGfnpKDgzJ8a+dXjqKMeooh/U9qWUVTmjXyRe8PdshB0ZkPXWWj+i
      ybpT2YlZ6oFo4KCU6/6fxPrg142Y72XE5ZgnZ29Ij86Mh67QWmgIhB0LonYfCjJr
      Ljhgvx+ajGlCcxmwcmXJVSZOkyFCwBChilrdAsayLokOfjbUmE+we6Og7bbDmUIb
      zE4lsuNqJFR0pUZV54UpaV+d6mNfjR/1r4b0B2WRxO0TmVinzNjiT6rpjrxbYeJK
      aEQ0wxnzX++dY2drUffqQGs4/aJn/CCF0QsiS7IdHtPVBZ058LPExfYqQfsJF6m6
      DGCJUA7pNjOSZJ8=
      -----END CERTIFICATE-----

      Certificate Fingerprint:
      1e6ebdafcb23d6e8512a2284719d6fdf2214ab19
      ```

    - Save all this information in a file or a safe place since we will need it later.

    - The key credentials that should be added to the manifest file are as follows from the output. **Copy the following from the terminal output**

      ```
      {
        "customKeyIdentifier": "Hm69r8sj1uhRKiKEcZ1v3yIUqxk=",
        "value": "MIID9zCCAt+gAwIBAgIBATANBgkqhkiG9w0BAQUFADB3MSgwJgYDVQQDEx9UcmF2aXMgVGlkd2VsbCA8dHJhdmlzQGZvcm0uaW8+MQswCQYDVQQGEwJVUzEOMAwGA1UECBMFVGV4YXMxDzANBgNVBAcTBkRhbGxhczEQMA4GA1UEChMHRm9ybS5pbzELMAkGA1UECxMCSVQwHhcNMTUwODEwMDMwMzQyWhcNMTYwODEwMDMwMzQyWjB3MSgwJgYDVQQDEx9UcmF2aXMgVGlkd2VsbCA8dHJhdmlzQGZvcm0uaW8+MQswCQYDVQQGEwJVUzEOMAwGA1UECBMFVGV4YXMxDzANBgNVBAcTBkRhbGxhczEQMA4GA1UEChMHRm9ybS5pbzELMAkGA1UECxMCSVQwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQC18uROe+X0/26zcgJTdluVd7tK4T407hGXTccNCebuKP2qUsh+/i0imLHLYZ3xs5hbSPPvjRF/wVm3CowZbnaCNNnd2WaUHFWBr+4ltTtxlpByPJIQ6rIMPvHkhfDUV05MN8ZvXUTZQEceVF/Y5dP/muB6v8zMeCRcEbvXNKHmDu8Ss6O5CdfXjoSwB4721NxmegHATdlet8JzaVal6/c4L5vzofVcu/wnF4f8bS2J4D5SBSh2WhpvMcjPsrrOmRfUIfR8rr7k4ZgBotJHsPwaCe/z10Q7K2Q3FpUpzgZ7t86gCVaDaEbR8aUMpEP2/Xhgk+T0nQIb1/Vh0dRIvDWJAgMBAAGjgY0wgYowDAYDVR0TBAUwAwEB/zALBgNVHQ8EBAMCAvQwOwYDVR0lBDQwMgYIKwYBBQUHAwEGCCsGAQUFBwMCBggrBgEFBQcDAwYIKwYBBQUHAwQGCCsGAQUFBwMIMBEGCWCGSAGG+EIBAQQEAwIA9zAdBgNVHQ4EFgQUnUysxhgEYmbnewKQOmWPcd1s7ncwDQYJKoZIhvcNAQEFBQADggEBAEKIfTm5SRDGM2KJ40cGGfnpKDgzJ8a+dXjqKMeooh/U9qWUVTmjXyRe8PdshB0ZkPXWWj+iybpT2YlZ6oFo4KCU6/6fxPrg142Y72XE5ZgnZ29Ij86Mh67QWmgIhB0LonYfCjJrLjhgvx+ajGlCcxmwcmXJVSZOkyFCwBChilrdAsayLokOfjbUmE+we6Og7bbDmUIbzE4lsuNqJFR0pUZV54UpaV+d6mNfjR/1r4b0B2WRxO0TmVinzNjiT6rpjrxbYeJKaEQ0wxnzX++dY2drUffqQGs4/aJn/CCF0QsiS7IdHtPVBZ058LPExfYqQfsJF6m6DGCJUA7pNjOSZJ8=",
        "keyId": "ea8ec579-fcbe-4ce5-ba65-2f5246dfdd6c",
        "usage": "Verify",
        "type": "AsymmetricX509Cert"
      }
      ```

    - You now need to save that and place it within the manifest file between the ```[]``` for the **keyCredentials** key. Like the following... *Make sure you keep the ```[]``` from the keyCredentials*

      ```
      ...
      ...
        "keyCredentials": [{
          "customKeyIdentifier": "Hm69r8sj1uhRKiKEcZ1v3yIUqxk=",
          "value": "MIID9zCCAt+gAwIBAgIBATANBgkqhkiG9w0BAQUFADB3MSgwJgYDVQQDEx9UcmF2aXMgVGlkd2VsbCA8dHJhdmlzQGZvcm0uaW8+MQswCQYDVQQGEwJVUzEOMAwGA1UECBMFVGV4YXMxDzANBgNVBAcTBkRhbGxhczEQMA4GA1UEChMHRm9ybS5pbzELMAkGA1UECxMCSVQwHhcNMTUwODEwMDMwMzQyWhcNMTYwODEwMDMwMzQyWjB3MSgwJgYDVQQDEx9UcmF2aXMgVGlkd2VsbCA8dHJhdmlzQGZvcm0uaW8+MQswCQYDVQQGEwJVUzEOMAwGA1UECBMFVGV4YXMxDzANBgNVBAcTBkRhbGxhczEQMA4GA1UEChMHRm9ybS5pbzELMAkGA1UECxMCSVQwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQC18uROe+X0/26zcgJTdluVd7tK4T407hGXTccNCebuKP2qUsh+/i0imLHLYZ3xs5hbSPPvjRF/wVm3CowZbnaCNNnd2WaUHFWBr+4ltTtxlpByPJIQ6rIMPvHkhfDUV05MN8ZvXUTZQEceVF/Y5dP/muB6v8zMeCRcEbvXNKHmDu8Ss6O5CdfXjoSwB4721NxmegHATdlet8JzaVal6/c4L5vzofVcu/wnF4f8bS2J4D5SBSh2WhpvMcjPsrrOmRfUIfR8rr7k4ZgBotJHsPwaCe/z10Q7K2Q3FpUpzgZ7t86gCVaDaEbR8aUMpEP2/Xhgk+T0nQIb1/Vh0dRIvDWJAgMBAAGjgY0wgYowDAYDVR0TBAUwAwEB/zALBgNVHQ8EBAMCAvQwOwYDVR0lBDQwMgYIKwYBBQUHAwEGCCsGAQUFBwMCBggrBgEFBQcDAwYIKwYBBQUHAwQGCCsGAQUFBwMIMBEGCWCGSAGG+EIBAQQEAwIA9zAdBgNVHQ4EFgQUnUysxhgEYmbnewKQOmWPcd1s7ncwDQYJKoZIhvcNAQEFBQADggEBAEKIfTm5SRDGM2KJ40cGGfnpKDgzJ8a+dXjqKMeooh/U9qWUVTmjXyRe8PdshB0ZkPXWWj+iybpT2YlZ6oFo4KCU6/6fxPrg142Y72XE5ZgnZ29Ij86Mh67QWmgIhB0LonYfCjJrLjhgvx+ajGlCcxmwcmXJVSZOkyFCwBChilrdAsayLokOfjbUmE+we6Og7bbDmUIbzE4lsuNqJFR0pUZV54UpaV+d6mNfjR/1r4b0B2WRxO0TmVinzNjiT6rpjrxbYeJKaEQ0wxnzX++dY2drUffqQGs4/aJn/CCF0QsiS7IdHtPVBZ058LPExfYqQfsJF6m6DGCJUA7pNjOSZJ8=",
          "keyId": "ea8ec579-fcbe-4ce5-ba65-2f5246dfdd6c",
          "usage": "Verify",
          "type": "AsymmetricX509Cert"
        }],
      ...
      ...
      ```

    - Now, to save the manifest file, click on ***Manifest***, then insert copied ***keyCredentials***.

    - Click on the ***Save*** button.

      ![](/assets/img/office365/v2/azure-update-manifest.png){: .img-fluid .img-thumbnail }

    - You are now ready to configure **&lt;<span class="text-primary">form</span>.<span class="text-secondary">io</span>&gt;** for **Office 365 Integration!**

  - ### Step 4: Configure Form.io for Office 365 Integration

    - Once you create an account on **&lt;<span class="text-primary">form</span>.<span class="text-secondary">io</span>&gt;**, and then create a Project, you will need to click on the **Settings** within your project, then click on the ***Integrations***, then click on the ***Data Connections***.

    - Next, you will need to click on the **Office 365** settings.

      ![](/assets/img/office365/v2/portal-open-settings.png){: .img-fluid .img-thumbnail }

    - Next, you will need to specify your Tenant ID and Application ID, which can be found by clicking on ***Overview*** on the Azure Application page and then copy IDs.

      ![](/assets/img/office365/v2/azure-app-properties.png){: .img-fluid .img-thumbnail }

    - Enter that information into your Office 365 settings, and also provide your Office 365 email address.

      ![](/assets/img/office365/v2/portal-apply-settings.png){: .img-fluid .img-thumbnail }

    - Congratulations, you are now ready for Office 365 integration within **&lt;<span class="text-primary">form</span>.<span class="text-secondary">io</span>&gt;**!

  - ## Step 5: Test out the integration

    - To test out the integration, we can create a simple "Contact Form", which provides **First Name**, **Last Name**, and **Email** as follows.

      ![](/assets/img/office365/v2/portal-contact-form.png){: .img-fluid .img-thumbnail }

    - Next, click on the **Actions** tab and then select the **Office 365 Contacts** action and then click on the **Add Action** button.

      ![](/assets/img/office365/v2/portal-add-action.png){: .img-fluid .img-thumbnail }

    - On the next page, we simply need to map the **First Name**, **Last Name**, and **Email** **&lt;<span class="text-primary">form</span>.<span class="text-secondary">io</span>&gt;** fields to the Office 365 **First Name Field**, **Last Name Field**, and **Email Address Field** select drop-downs respectively...

      ![](/assets/img/office365/v2/portal-map-fields.png){: .img-fluid .img-thumbnail }

    - Next, press **Save** to create the Action for your Form.

  - ## What did we just do?
    This just created a mapping to the **&lt;<span class="text-primary">form</span>.<span class="text-secondary">io</span>&gt;** forms to your Office 365 Contacts section. You should now be able to **Create**, **Update**, and **Delete** Office 365 Contacts from within the **&lt;<span class="text-primary">form</span>.<span class="text-secondary">io</span>&gt;** application interface.

    Now, go to your **&lt;<span class="text-primary">form</span>.<span class="text-secondary">io</span>&gt;** form, and then submit a new contact using the **&lt;<span class="text-primary">form</span>.<span class="text-secondary">io</span>&gt;** form.

      ![](/assets/img/office365/v2/portal-launch.png){: .img-fluid .img-thumbnail }

    Once you press **Submit** on the form, you should now be able to see the data within **&lt;<span class="text-primary">form</span>.<span class="text-secondary">io</span>&gt;**

      ![](/assets/img/office365/v2/portal-submission.png){: .img-fluid .img-thumbnail }
      ![](/assets/img/office365/v2/outlook-submission.png){: .img-fluid .img-thumbnail }

  - ## More Information
    We will be adding more capabilites to the Office 365 integration in the future. We will update this list with more information as that documentation is written. Enjoy!
