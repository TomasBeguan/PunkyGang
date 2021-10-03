<template>
    <div id="app">

  <div class="container-fluid">
    <div class="row">
      <div class="col-md-3">
        <h3>Locations</h3>
        <ul>
          <li v-for="location in locations">
           <input type="checkbox" v-model="checkedLocations" v-bind:value="location" />  {{location}}
          </li>
        </ul>
        <span>Checked locations: {{ checkedLocations }}</span>
      </div>
      <div class="col-md-9">
         <div class="jobs">
           <div class="job" v-for="job in filteredJobs">
             <div class="card" style="margin: 5px 0">
               <div class="card-block">
                 <h4 class="card-title">
                   {{ job.title }}
                 </h4>
                 <small class="pull-right text-muted" style="font-size: 12x">
                   {{job.date}}
                 </small>
                 <p class="card-text">
<!--                    {{job.description}} -->
                 </p>
                 <button class="btn btn-sm btn-primary">View</button>
               </div>
             </div>
           </div>
         </div>
      </div>
    </div>
  </div>

</div>
</template>

<script>


console.clear()

let careers = careers || {};

const jobs = [
              { id: '1', title: 'Circuit Court Clerk', description: 'lorem ipsum', location: 'Charlotte, NC', department: 'Sales and Marketing', date: '23/10/17' },
              { id: '2', title: 'Tie Buyer', description: 'lorem ipsum', location: 'Irvine, CA', department: 'Media Relations', date: '21/10/16' },
              { id: '3', title: 'Leaded Glass Installer', description: 'lorem ipsum', location: 'Kansas City, MO', department: 'Public Relations', date: '16/09/17' },
              { id: '4', title: 'Wheat Inspector', description: 'lorem ipsum', location: 'Red Bank, NJ', department: 'Quality Assurance', date: '12/06/16' },
              { id: '5', title: 'Executive Officer, Special Warfare Team', description: 'lorem ipsum', location: 'Irving, TX', department: 'Public Relations', date: '19/03/18' },
              { id: '6', title: 'Wildlife Control Agent', description: 'lorem ipsum', location: 'Irvine, CA', department: 'Sales and Marketing', date: '07/01/17' },
              { id: '7', title: 'Arresting Gear Operator', description: 'lorem ipsum', location: 'Charlotte, NC', department: 'Asset Management', date: '04/09/17' },
              { id: '8', title: 'Arresting Gear Operator', description: 'lorem ipsum', location: 'Chandler, AZ', department: 'Tech Support', date: '01/04/17' },
              { id: '9', title: 'Biogeographer', description: 'lorem ipsum', location: 'Chicago, IL', department: 'Quality Assurance', date: '01/05/17' },
              { id: '10', title: 'LAN Systems Administrator', description: 'lorem ipsum', location: 'Scottsdale, AZ', department: 'Customer Relations', date: '29/04/18' },
              { id: '11', title: 'Copper Plater', description: 'lorem ipsum', location: 'Irving, TX', department: 'Tech Support', date: '17/08/17' },
              { id: '12', title: 'Leaded Glass Installer', description: 'lorem ipsum', location: 'Rockville, MD', department: 'Sales and Marketing', date: '02/11/16' },
              { id: '13', title: 'Line Cook', description: 'lorem ipsum', location: 'Chandler, AZ', department: 'Advertising', date: '02/12/17' },
              { id: '14', title: 'Special Education Teaching Assistant', description: 'lorem ipsum', location: 'Red Bank, NJ', department: 'Payroll', date: '02/05/17' },
              { id: '15', title: 'Clarinetist', description: 'lorem ipsum', location: 'Melville, NY', department: 'Payroll', date: '30/05/17' },
              { id: '16', title: 'Arresting Gear Operator', description: 'lorem ipsum', location: 'Henderson, NV', department: 'Payroll', date: '23/02/18' },
              { id: '17', title: 'Wheat Inspector', description: 'lorem ipsum', location: 'Red Bank, NJ', department: 'Tech Support', date: '12/08/16' },
              { id: '18', title: 'Wildlife Control Agent', description: 'lorem ipsum', location: 'Melville, NY', department: 'Payroll', date: '03/05/17' },
              { id: '19', title: 'Print Retoucher', description: 'lorem ipsum', location: 'Chicago, IL', department: 'Sales and Marketing', date: '19/06/16' },
              { id: '20', title: 'Mathematical Statistician', description: 'lorem ipsum', location: 'Scottsdale, AZ', department: 'Tech Support', date: '10/07/16' }
            ];

 careers.taleo = (function($){

   let app;

   let init = function() {

  export default {
    name: 'Prueba',
    
    el: '#app',
    data: {
        locations: ['Scottsdale, AZ','Chandler, AZ','Irvine, CA','Denver, CO','Chicago, IL','Rockville, MD','Kansas City, MO','Charlotte, NC','Red Bank, NJ','Henderson, NV','Melville, NY','Allentown, PA','Irving, TX'],
        checkedLocations: []
      },
      created: function () {
        this.fetchData();
      },
      computed:{
        filteredJobs(){
          if (!this.checkedLocations.length)
              return this.jobs
          
          // return this.jobs.reduce((a, j)=> {
          //   if (this.checkedLocations.includes(j.location))
          //     a.push(j)
          //   return a
          //  },[])
          return this.jobs.filter(j => this.checkedLocations.includes(j.location))
        }
      },
      methods: {
          fetchData: function () {
            this.jobs = jobs
          }
        }
    }



   };


   return { init, app };

})();

(function(){
  careers.taleo.init();
})();


  

</script>