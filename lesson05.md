## Building the Client (Part 1)

We will be creating three pages:  
### Home Page  
<img src="Homepage.png" alt="drawing" width="200"/>

### Poll Page
<img src="Pollpage.png" alt="drawing" width="200"/>

### Results Page
<img src="Resultspage.png" alt="drawing" width="200"/>

In VSCode start a new **Terminal** and change the directory to **client**:
```
cd client
```

Now we install the graphics package that will help us display the chart. In the Terminal enter:
```
npm i vue-google-charts
```

Download images:  
https://candogram-downloads.s3.amazonaws.com/student-poll-images.zip  


Before we begin we need to clean up the default app by deleting some files and folders:  
- /public => delete all file(s) and folder(s)
- /src => delete the **assets** folder
- /src/components => delete all file(s)
- /src/pages => delete IndexPage.vue

From the unzipped images
- copy the folder **icons** and **favicon.ico** to the **/public** folder.
- copy the folder **assets** to the **/src** folder


Open the page /src/layouts/MainLayout.vue. Delete all contents. Add the following code:
```
<template>
  <q-layout view="lHh Lpr lFf">
    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>

export default {
  name: 'MainLayout'
}
</script>
```
Close the MainLayout.vue file.
Click on the **pages** folder and create three files:
- HomePage.vue
- PollPage.vue
- ResultsPage.vue

Doubleclick the file **quasar.config.js**. Find the line **vueRouterMode: 'hash'**. Replace **hash** with **history**.

Underneath this entry enter the following code:
```
      env: {
        BASE_URL: 'http://localhost:3000', 
        CREATOR_NAME: 'Henning Seip',
        CREATOR_EMAIL: 'henning.seip@candogram.com'
      },
      distDir: '../server/public'
 ```     
For the CREATOR_NAME and CREATOR_EMAIL variables use your own information.

