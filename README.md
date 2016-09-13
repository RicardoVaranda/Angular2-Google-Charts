Angular2 CLI Google Charts
=========

###1.In index.html page include following script in the <header> tags
    <script src="https://www.gstatic.com/charts/loader.js"></script>
    <script>  
    !important: You want to give this variable(var googleLoaded = false;). This is used to run multiple chart in your jade.
      var googleLoaded = false;
      </script>
###2.Create / Generate component
####if you are using angular-cli do the following command in app folder
    ng g component GoogleChart
###3.Copy this inside the GoogleChartComponent generated
    import {Directive,ElementRef,Input,OnInit} from '@angular/core';
    declare var google:any;
    declare var googleLoaded:any;
    @Directive({
      selector: '[GoogleChart]'
    })
    export class GoogleChartComponent implements OnInit {
      public _element:any;
      @Input('chartType') public chartType:string;
      @Input('chartOptions') public chartOptions: Object;
      @Input('chartData') public chartData: Object;
      constructor(public element: ElementRef) {
        this._element = this.element.nativeElement;
      }
      ngOnInit() {
        setTimeout(() =>{
          google.charts.load('current', {'packages':['corechart']});
            setTimeout(() =>{
              this.drawGraph(this.chartOptions,this.chartType,this.chartData,this._element)
            },1000);
          },1000
        );
      }
      drawGraph (chartOptions,chartType,chartData,ele) {
        google.charts.setOnLoadCallback(drawChart);
        function drawChart() {
          var wrapper;
          wrapper = new google.visualization.ChartWrapper({
            chartType: chartType,
            dataTable:chartData ,
            options:chartOptions || {},
            containerId: ele.id
          });
          wrapper.draw();
        }
      }
    }
###4 In the component you want to include a chart do the following
    import {Component, OnInit } from '@angular/core';
    import {GoogleChartComponent} from '../google-chart/google-chart.component';
    
    @Component({
    
      selector: 'app-dashboard-vapp-pie',
      templateUrl: 'dashboard-vapp-pie.component.html',
      styleUrls: ['dashboard-vapp-pie.component.css'],
    })
    export class DashboardVappPieComponent implements OnInit {
    
      public pie_ChartData = [
        ['Task', 'Hours per Day'],
        ['Work',     11],
        ['Eat',      2],
        ['Commute',  2],
        ['Watch TV', 2],
        ['Sleep',    7] ];
      public pie_ChartOptions  = {
        title: 'My Daily Activities',
        width: 900,
        height: 500
      };
    
      constructor() {
      }
    
      ngOnInit() {
    
      }
    
    }
###5 In the html use it like this
    <div id="pie_chart" [chartData]="pie_ChartData" [chartOptions] = "pie_ChartOptions" chartType="PieChart" GoogleChart></div>

## Demo Output
  ![angular2-google-chart-v1 0 5](https://cloud.githubusercontent.com/assets/11042288/16228824/f30a4436-37d6-11e6-8d05-bdc8bc090fcc.png)
