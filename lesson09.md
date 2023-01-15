## Building the Client (Part 4)

Doubleclick on the file ResultsPage.vue and add the following code:
```
<template>
  <div>
    <div class="row">
      <div class="col-8 bg-grey-1" style="margin:auto;border:1px solid #dddddd">
        <h3 class="gt-sm text-center q-mt-md q-mb-none">Poll Results</h3>
        <h4 class="lt-md text-center text-bold q-mt-md q-mb-none">Poll Results</h4>
        <h5 class="text-center q-ma-none">for Bronx Academy for Software Engineering</h5>
        <GChart type="BarChart" :data="chartData" :options="chartOptions" :style="chartStyle" />
        <div style="width:100%; text-align: right">
          <q-btn color="secondary" :to="{ name: 'Home' }">Home</q-btn>
        </div>
      </div>
    </div>
   
  </div>
</template>

<script>
import { GChart } from 'vue-google-charts'
export default {
  components: {
    GChart
  },
  data() {
    return {
      error: false,
      chartOptions: {
        backgroundColor: 'transparent',
        legend: { position: 'none' },
        hAxis: { format: '0' }
      },
      chartStyle: 'height: calc(70vh)',
      chartData: []
    }
  },
  mounted() {
    fetch(process.env.BASE_URL + '/api/results')
      .then(response => response.json())
      .then(data => {
        let chartData = []
        chartData.push(['Job Title', 'Students', { role: 'style' }])
        for (const rec of data) {
          chartData.push([
            rec.JobTitle,
            parseInt(rec.studentCount),
            '#0000FF'
          ])
        }
        this.chartData = chartData
      })
  }
}
</script>
```

Let's test the software!  
Click into the **client** Terminal and enter:
```
quasar dev
```
The application will start showing the home screen.  
Enter an BASE email address and hit the **GO** button.  
On the next screen select a job title and scroll all the way down and click the **SAVE YOUR CHOICE** button.  
Next click the **SHOW POLL RESULTS** button.  You will see the bar chart. 

Finally we prepare our project for production.  
In VSCode open the empty **appspec.yml** file and enter the following code:
```
version: 0.0
os: linux
files:
  - source: /
    destination: /home/ubuntu/BASE
hooks:
  AfterInstall:
    - location: AWS_install_scripts/installApp.sh
      timeout: 600
      runas: root
```

Click on the **AWS_install_scripts** directory and create a new file called **installApp.sh**. This will be a shell script that will install our software on the production server.  

Enter the following code:
```
#!/bin/bash

#----------------------------------
# install the node server
#----------------------------------
cp /home/ubuntu/env/env /home/ubuntu/BASE/server/.env

cd /home/ubuntu/BASE/server
npm install

if [ ! -d "$public" ]; then
  # Control will enter here if $public doesn't exist.
  mkdir public
fi


#----------------------------------
# build the client app
#----------------------------------
cd /home/ubuntu/BASE/client
npm install
quasar build

#start Node server.
systemctl start nodejs
```




