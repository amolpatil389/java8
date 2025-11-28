Style.css

@import url(https://unpkg.com/bootstrap@4.1.0/dist/css/bootstrap.min.css);
************************************************************************************************
app.html

<app-menu></app-menu>
    <div class="container">
        <router-outlet></router-outlet>
    </div>
<app-footer></app-footer>
<!-- <app-welcome></app-welcome> -->

******************************************************************************************************
Welcome.html

<H1>Welcome</H1>
<div class="container">
    Welcome <b>{{name | lowercase}}</b>,You Can Manage your todos <a routerLink="/todos">here</a>
</div> 

*****************************************************************************************************
welcome.ts

import { LowerCasePipe, UpperCasePipe } from '@angular/common';
import { Component, OnInit, Pipe } from '@angular/core';
import { ActivatedRoute, RouterLink } from '@angular/router';


@Component({
  selector: 'app-welcome',
  standalone: true,
  imports: [RouterLink,LowerCasePipe],
  templateUrl: './welcome.html',
  styleUrl: './welcome.css',
})


export class Welcome implements OnInit{

  name = '';

  constructor(private route:ActivatedRoute){}
  ngOnInit(): void {
   console.log(this.route.snapshot.params['name']) 
  this.name =  this.route.snapshot.params['name']
  }

  


}
*****************************************************************************************************

menu.html

<header>
    <nav class="navbar navbar-expand-md navbar-dark bg-dark">
        <div><a href="http://localhost:4200/" class="navbar-brand">localhost</a></div>

        <ul class="navbar-nav">
            <li><a routerLink="/welcome/admin" class="nav-link">Home</a></li>
            <li><a routerLink="/todos" class="nav-link">Todos</a></li>
        </ul>



        <ul class="navbar-nav navbar-collapse justify-content-end">
            <li><a routerLink="/login" class="nav-link">Login</a></li>
            <li><a routerLink="/logout" class="nav-link">Logout</a></li>
        </ul>


    </nav>

</header>
************************************************************************************************

menu.ts

import { Component } from '@angular/core';
import { RouterLink } from '@angular/router';

@Component({
  selector: 'app-menu',
  imports: [RouterLink],
  templateUrl: './menu.html',
  styleUrl: './menu.css',
})
export class Menu {

}
**********************************************************************************************************

login.html

<h1>Login</h1>
<div class="container">
    <div class="alert alert-warning" *ngIf="invalidLogin">{{errorMessage}}</div>
    <div>
        User Name : <input type="text" name="username" [(ngModel)]="username">
        Password : <input type="password" name="password" [(ngModel)]="password">
        <!-- Username : {{username}} -->
        <button type="button" (click)=handleLogin() class="btn btn-success">Login</button>

    </div>
</div>
*********************************************************************************************************

login.ts

import { CommonModule, NgIf } from '@angular/common';
import { Component } from '@angular/core';
import { FormsModule } from '@angular/forms';
import { Router } from '@angular/router';


@Component({
  selector: 'app-login',
  imports: [FormsModule,CommonModule],
  templateUrl: './login.html',
  styleUrl: './login.css',
})
export class Login {

  username = 'admin'
  password = ''
  errorMessage = 'Invalid Credentials'
  invalidLogin = false

  constructor(private router : Router)
  { }

  handleLogin(){

    if (this.username==='admin' && this.password==='dummy')
    {
      this.router.navigate(['welcome',this.username])
      this.invalidLogin = false
    }
  else
  {
    this.invalidLogin = true
  }
  }

}
*******************************************************************************************************************

list-todos.html

<H1> My Todo's</H1>

<div class="container">
    <table class="table">

        <thead>
            <tr>
                <!-- <th>id</th> -->
                <th>description</th>
                <th>Target Date</th>
                <th>Is Completed?</th>

            </tr>
        </thead>
        <tbody>
            <tr *ngFor="let todo of todos">
                <!-- <td>{{todo.id}}</td> -->
                <td>{{todo.description}}</td>
                <td>{{todo.targetDate | date | uppercase}}</td>
                <td>{{todo.done}}</td>
            </tr>
        </tbody>
    </table>
</div>
*************************************************************************************************************************

list-todos.ts

import { CommonModule, DatePipe } from '@angular/common';
import { Component } from '@angular/core';

export class Todo{
  constructor(
    public id: number,
    public description: string,
    public done : boolean,
    public targetDate : Date
  ){

  }
}

@Component({
  selector: 'app-list-todos',
  imports: [CommonModule,DatePipe],
  // DatePipe
  templateUrl: './list-todos.html',
  standalone: true,
  styleUrl: './list-todos.css',
})
export class ListTodos {

  todos = [
    new Todo(1,'Learn Angular',false,new Date()),
    new Todo(2,'Learn Spring',false,new Date()),
    new Todo(3,'Learn AWS',false,new Date())


    // { id: 1, description: 'Learn Angular' },
    // { id: 2, description: 'Build a project' },
    // { id: 3, description: 'Visit India' },

  ]
}
*******************************************************************************************************************
footer.html

<footer class="footer">
    <div class="container">
     <span class="text-muted">All Rights Reserved 2025 @amolpatil</span>   
    </div>
</footer>

********************************************************************************************************************

footer.css

.footer{
    position: absolute;
    bottom: 0;
    width: 100%;
    height: 40px;
    background-color: #222222;
}

**********************************************************************************************************************

app.routes.ts

import { Routes } from '@angular/router';
import { Login } from './login/login';
import { Welcome } from './welcome/welcome';
import { Error } from './error/error';
import { ListTodos } from './list-todos/list-todos';

export const routes: Routes = [

    {path:'',component:Login},
    {path:'login',component:Login},
    {path:'welcome/:name',component:Welcome},
    {path:'todos',component:ListTodos},
    {path:'**',component:Error}
];

****************************************************************************************************************************



