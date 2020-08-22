# laravel0controller Hotels.php<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use App\hotel;
use App\user1;
use Session;
use Crypt;
class Hotels extends Controller
{
    function index()
    {
       return view('home');
    }
    function list()
    {
       $data=  hotel::all();
       return view('list',['data'=>$data]);
    }
    function add(Request $req)
    {
      //
      //$data=  hotel::all($req);
       //return $req->input();
       $hotel = new hotel;
      $hotel->name=$req->name;
      $hotel->gmail=$req->gmail;
      $hotel->address=$req->address;
      $hotel->save();
      $req->session()->flash('status','hotel submitted succesfully');
      return redirect('list');

    }
    function delete($id)
    {
        echo hotel::find($id)->delete();
        Session::flash('status','hotel delete succesfully');
        return redirect('list');
    }
    function edit($id)
    {
        $data= hotel::find($id);
        return view('edit',['data'=>$data]);
    }
    function update(Request $req)
    {
      //
      //$data=  hotel::all($req);
       //return $req->input();
       $hotel = hotel::find($req->input('id'));
      $hotel->name=$req->input('name');
      $hotel->gmail=$req->input('gmail');
      $hotel->address=$req->input('address');
      $hotel->save();
      $req->session()->flash('status','hotel updated succesfully');
      return redirect('list');

    }
    function register(Request $req)
    {
        //echo Crypt::encrypt('123@abc');
        //return $req->input();
        $user1 = new user1;
        $user1->name=$req->input('name');
        $user1->gmail=$req->input('gmail');
        $user1->password=Crypt::encrypt($req->input('password'));
        $user1->save();
        $req->session()->put('user',$req->input('name'));
       return redirect('/');
  
    }
    function login(Request $req)
    {
        $user1=user1::where("gmail",$req->input('gmail'))->get();
        if(Crypt::decrypt($user1[0]->password)==$req->input('password'))
        {
            $req->session()->put('user',$user1[0]->name);
            return redirect('/');
        }
    }
}
user1.php model
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class user1 extends Model
{
    
}
add,blade,php
@extends('layout')
@section('content')
<div class="col-sm-6">
<h1>Add new Hotels</h1>
<form method="post" action="add">
@csrf
  <div class="form-group">
    <label>Name</label>
    <input type="name" class="form-control"  placeholder="Enter name" name="name">
  </div>
  <div class="form-group">
    <label>Email address</label>
    <input type="gmail" class="form-control"  placeholder="Enter email" name="gmail">
  </div><div class="form-group">
    <label>Address</label>
    <input type="address" class="form-control"  placeholder="Enter address" name="address">
  </div>
  
  <button type="submit" class="btn btn-primary">Submit</button>
</form>



</div>
@stop
home.blade.php
@extends('layout')
@section('content')
<div>
<h1>Home Page</h1>
</div>
@stop
hotel.php
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class hotel extends Model
{
    //
}
layout.blade.php
<!DOCTYPE html>
<html>
<head>
	<title>Hotel App</title>
  
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css">
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css" integrity="sha384-Gn5384xqQ1aoWXA+058RXPxPg6fy4IWvTNh0E263XmFcJlSAwiGgFAW/dAiS6JXm" crossorigin="anonymous">
    <script src="https://code.jquery.com/jquery-3.2.1.slim.min.js" integrity="sha384-KJ3o2DKtIkvYIK3UENzmM7KCkRr/rE9/Qpg6aAZGJwFDMVNA/GpGFF93hXpG5KkN" crossorigin="anonymous"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.12.9/umd/popper.min.js" integrity="sha384-ApNbgh9B+Y1QKtv3Rn7W3mgPxhU9K/ScQsAP7hUibX39j7fakFPskvXusvfa0b4Q" crossorigin="anonymous"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/js/bootstrap.min.js" integrity="sha384-JZR6Spejh4U02d8jOt6vLEHfe/JQGiRRSQQxSfFWpi1MquVdAyjUar5+76PVCmYl" crossorigin="anonymous"></script>
</head>
<body> 
<header>
<nav class="navbar navbar-expand-sm navbar-light bg-light">
  <a class="navbar-brand" href="#">Hotel</a>
  <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNavAltMarkup" aria-controls="navbarNavAltMarkup" aria-expanded="false" aria-label="Toggle navigation">
    <span class="navbar-toggler-icon"></span>
  </button>
  <div class="collapse navbar-collapse" id="navbarNav">
  <ul class="navbar-nav"
    <li class="nav-item active">
      <a class="nav-link" href="/">Home <span class="sr-only">(current)</span></a>
      </li>
      <li class="nav-item">
      <a class="nav-link" href="/list">List</a>
      </li>
      <li class="nav-item">
      <a class="nav-link" href="add">Add</a>
      </li>
      <li class="nav-item">
      <a class="nav-link" href="#">Search</a>
      <li class="nav-item">
      <a class=" nav-link" href="login">Log in</a>
      </li>
      <li class="nav-item">
      <a class="nav-link" href="register">Register</a>
      </li>
     
      @if(Session::get('user'))
      <li clas="nav-item">
      <a class="nav-link" href="#">Welcome|{{Session::get('user')}}</a>
      </li>
      @else
      <li class="nav-item">
      <a class=" nav-link" href="login">Log in</a>
      </li>
      <li class="nav-item">
      <a class="nav-link" href="register">Register</a>
      </li>
      @endif
      </ul>
  </div>
</nav>
</header>
<div class="container">
		@yield('content')
	</div>
<footer><!--Copy right by Hotel App--></footer>
</body>
</html>
