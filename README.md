# env-sops

## Description
env-sops is a utility package designed to encrypt sensitive environment variables, ensuring secure storage within code repositories on platforms like GitHub. This package internally utilizes SOPS+AGE for robust encryption and decryption, coupled with the convenience of using dotEnv to load variables after decryption.

## Installation
### Prerequisites

Before using `env-sops`, ensure that you have SOPS and AGE installed on your machine. These tools are essential for encryption and decryption processes within the package.

- **SOPS:** Provides robust encryption capabilities, enabling secure handling of secrets.
- **AGE:** A tool used in combination with SOPS for encryption and decryption functionalities.

Make sure to install and configure both SOPS and AGE to leverage the encryption and decryption features seamlessly offered by `env-sops`.
Instructions on how to install the package using NPM.

## Usage
### Create/encrypt/load environment variables
1. Install the package using NPM
```
npm install -D env-sops
```

2. Create File for Storing Environment Variables

```
touch encrypted-dev.env
```

Then edit the file and add some environment variables

```
DB_HOST=localhost
DB_USER=root
DB_PASS=s1mpl3
```


3. Add the Scripts for Encrypting and Decrypting Environment Variables file into package.json
``` JSON
(...)
"type":"module",
"scripts": {
    "start": "node main.js",
    "sops:encrypt:dev": "envencrypt encrypted-dev.env",
    "sops:decrypt:dev": "envdecrypt encrypted-dev.env ./"
},
(...)
```

4. Run Encrypt Script
The command below will encrypt the file and save it as encrypted-dev.env.
```
npm run sops:encrypt:dev
```

5. Read Environment Variables in Code
Edit your entrypoint script for loading decrypt and loading the environment variables

``` JavaScript
// main.js
import { loadEnvs } from 'env-sops'

const ENVS = {
    "dev": "encrypted-dev.env",
    "ut": "encrypted-ut.env",
    "prod": "encrypted-prod.env",
}

loadEnvs(ENVS[process?.env?.env] || ENVS['dev'])
    .then(() => {
        console.info('Envs loaded:')
        console.info(`DB_HOST: ${process?.env?.DB_HOST}`)
        console.info(`DB_PASS: ${process?.env?.DB_PASS}`)
        console.info(`DB_USER: ${process?.env?.DB_USER}`)
    })
    .catch(err => console.error(err))
```

6. Run Your Code
```
npm start
```

7. Commit the encrypted file to your repository for secure storage and version control.
``` bash
## Exclude the .env file from git
echo '.env' >> .gitignore
git add . 
git commit -m "Committing encrypted environment variables"
```


### Add/Edit environment variables file
1. Decrypt Environment Variables File to .env File
```
npm run sops:decrypt:dev
```
2. Copy the Decrypted File to the Original Location and then Edit the File
``` bash
cp -f  .env ./encrypted-dev.env
## open and edit the file encrypted-dev.env wirh your new values
```
3. Run Encrypt Script
```
npm run sops:encrypt:dev
```


### Decrypt environment variables file
1. Decrypt Environment Variables File to .env File
``` bash
npm run sops:decrypt:dev
## Then open the file .env and check the values
```


### Speed Coding
Youtube Video Soon at [baikenlabs](https://www.youtube.com/@baikenlabs)

## License
This project is licensed under the Apache License 2.0 - see the [LICENSE](LICENSE) file for details.

## Changelog
Initial Version

## Support/Contact
Provide ways for users to seek support, report vulnerabilities, or contribute to discussions related to the package's security.

## Acknowledgements
This project utilizes the following third-party tools and libraries:

- [SOPS](https://github.com/getsops/sops): Used for robust encryption and decryption of sensitive data within the environment variables.
- [AGE](https://github.com/FiloSottile/age): Integrated with SOPS for secure encryption and decryption functionalities.
- [dotEnv](https://github.com/motdotla/dotenv): Utilized for managing environment variables after decryption, facilitating easy integration into the application.
