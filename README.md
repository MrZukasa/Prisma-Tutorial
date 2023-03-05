# Prisma

[Qui](https://www.youtube.com/watch?v=RebA5J-rlwg&t=2832s) ce il video che sto usando per capire come funziona prisma e fare un po' di documentazione.

Tanto per cominciare per installare il tutto è necessario installare come locali, lse seguenti dipendenze:

```pws
npm i --save-dev prisma typescript ts-node @types/node nodemon
```

oppure

```pws
yarn add prisma typescript ts-node @types/node nodemon -D
```

Creare un file `tsconfig.json` con dentro:

```json
{
    "compilerOptions": {
        "sourceMap": true,
        "outDir": "dist",
        "strict": true,
        "lib": ["ESNext"],
        "esModuleInterop": true
    }
}
```

Una volta settato il `tsconfig.json` possso inizializzare prisma con :

```pwsh
npx prisma init --datasource-provider postgresql
```

>in questo caso utilizzo il database che usa nel tutorial, però per il mio progetto usero mongodb...forse

Questo comando genera uno `schema.prisma` con un generator e un datasource.
Genera anche un file `.env` che contiene l'URL del database.

Nel file `.env` ho messo user, password ed il nome del db.
Chiaramente il db va creato prima di potersi connettere.

Creo un `model` con un paio di campi e lo applico al mio database con una migrazione usando:

```pwsh
npx prisma migrate dev --name init
```

Fondamentalmente dico che migro il database con questo schema ed è utilizzato solo per l'ambiente di `dev` e si chiamera `init` che sta per initialize, dato che è uno schema di inizializzazione.

Installo anche `npm i @prisma/client` e poi genero il primsa client `npx prisma generate` che verrà messo dentro le node_modules, cosi da poter scrivere typescript per creare lo schema che creerà la struttura del db.

Adesso creo il file .ts che nel mio caso si chiamerà `script.ts` e ci copio dentro

```ts
import { PrismaClient } from "@prisma/client";
const prisma = new PrismaClient();

async function main() {}

main()
  .catch((e) => {
    console.log(e.message);
  })
  .finally(async () => {
    await prisma.$disconnect();
  });
```

Questo script consente di avere un metodo main, uno catch degli errori e in fine sempre una disconnessione dal db.

Le istruzioni che questo script farà devono andare nel metodo main.
>devo ricordarmi di creare uno script di partenza con l'esecuzione di nodemon dentro il file package.json per poter eseguire il file.ts

Fondamentalmente cosi facendo, posso scrivere typescript nel BE e avere delle query direttamente dentro il mio db, che sia SQL o NONSQL poco cambia, dato che tanto posso aggiornare il mio schema quando mi pare.
