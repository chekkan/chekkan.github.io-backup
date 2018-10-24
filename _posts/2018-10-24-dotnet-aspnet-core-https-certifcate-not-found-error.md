---
title: 'dotnet aspnet core https certifcate not found error '
layout: post
date: 2018-10-24 00:00:00 +0000
tags:
- dotnet
- aspnet
- kestrel
- mac
permalink: dotnet-aspnet-core-https-certifcate-not-found-error

---
I came across this particular issue when my Mac's account was changed from a domain associated account to a local user account.

Among many other things, my dotnet aspnet core web api was failing to run at the start with the error that said "Unable to configure HTTPS endpoint. No server certificate was specified, and the default developer certificate could not be found.". The output also has friendly message telling you exactly what to do as well. 

> To generate a developer certificate run 'dotnet dev-certs https'. To trust the certificate (Windows and macOS only) run 'dotnet dev-certs https --trust'.

Running the command `dotnet dev-certs https` however returned a message that said "A valid HTTPS certificate is already present.".

You will also get to know that the user profile location is at `~/.aspnet/DataProtection-Keys`. For me, this location already had a few `key-_.xml` files. So, I did made the decision to delete the files. Ran the dev-certs https command again. But again I got the message saying the "A valid HTTPS certificate is already present". I found there was an option that you could pass to the `dotnet dev-certs https` command, the `--clean` option. When I ran this, I got the message "Cleaning HTTPS development certificates from the machine.". But, I could find the `key-_.xml` files again in the `~/.aspnet/DataProtection-Keys` directory. 

Turns out, that you will need to delete the certificate from the Keychain Access manually as well in order to completely remove the self-signed certificates for localhost. 

Running `dotnet dev-certs https` command creates a localhost certificate in your logins section in the Keychain Access app. 

![](/uploads/Screen Shot 2018-10-24 at 10.23.08.png)

Running `dotnet dev-certs https --trust` command creates a trusted root certificate into your System's store.

![](/uploads/Screen Shot 2018-10-24 at 10.24.49.png)

So, if you run into this same issue, make sure to delete the certificate from your **system Certificates** store and also from the **login** keyschain. And run `dotnet dev-certs https` command followed by `dotnet dev-certs https --trust` command.