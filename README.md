<script>
let students = JSON.parse(localStorage.getItem('fortune') || '{}');
let current = '';

function genReg(){
 return 'CB1/'+Math.floor(1000+Math.random()*9000)+'/26';
}

function save(){
 localStorage.setItem('fortune', JSON.stringify(students));
}

function registerStudent(){
 let r = genReg();
 let name = sname.value;

 if(!name){ alert("Enter student name"); return; }

 let file = photo.files[0];

 if(file){
   let reader = new FileReader();
   reader.onload = ()=>{
     students[r]={name:name,course:course.value,pass:'1234',fees:'Not set',results:'',photo:reader.result};
     save();
     regshow.innerText="Registered: "+r+" Password:1234";
   };
   reader.readAsDataURL(file);
 } else {
   students[r]={name:name,course:course.value,pass:'1234',fees:'Not set',results:'',photo:''};
   save();
   regshow.innerText="Registered: "+r+" Password:1234";
 }
}

function loginStudent(){
 let r=lreg.value;
 let p=lpass.value;

 if(students[r] && students[r].pass===p){
 current=r;
 signup.style.display='none';
 login.style.display='none';
 studentDash.style.display='block';

 welcome.innerText='Welcome '+students[r].name;
 showReg.innerText=r;
 pimg.src=students[r].photo;
 aname.innerText=students[r].name;
 areg2.innerText=r;
 acourse.innerText=students[r].course;
 showFees.innerText='Balance: '+students[r].fees;
 showResults.innerText=students[r].results;

 pid.src=students[r].photo;
 idname.innerText=students[r].name;
 idreg.innerText=r;

 calcGPA();
 } else alert("Wrong login");
}

function forgotPassword(){
 let r=prompt("Enter Reg");
 if(students[r]){
 students[r].pass=r;
 save();
 alert("Password reset to Registration Number");
 }
}

function showAdminLogin(){
 signup.style.display='none';
 login.style.display='none';
 adminLogin.style.display='block';
}

function loginAdmin(){
 if(adminUser.value==='admin' && adminPass.value==='admin'){
 adminLogin.style.display='none';
 adminDash.style.display='block';
 loadTable();
 } else alert("Wrong admin login");
}

function saveAdmin(){
 let r=areg.value;
 if(students[r]){
 students[r].fees=abal.value;
 students[r].results=units.value;
 save();
 loadTable();
 }
}

function saveSemester(){
 students[current].semester=sem.value;
 students[current].units=unitsel.value;
 save();
 alert("Semester Registered");
}

function calcGPA(){
 let res=students[current].results.split(',');
 let total=0,c=0;
 res.forEach(x=>{
 let m=parseInt(x.split(':')[1]);
 if(m){total+=m;c++;}
 });
 if(c>0) gpa.innerText=(total/c/20).toFixed(2);
}

function loadTable(){
 studentTable.innerHTML='<tr><th>Reg</th><th>Name</th><th>Course</th><th>Fees</th><th>Results</th></tr>';
 let q=search.value||'';
 for(let r in students){
 if(r.includes(q))
 studentTable.innerHTML+=`<tr><td>${r}</td><td>${students[r].name}</td><td>${students[r].course}</td><td>${students[r].fees}</td><td>${students[r].results}</td></tr>`;
 }
}

function exportExcel(){
 let csv='Reg,Name,Course,Fees,Results\n';
 for(let r in students){
 csv+=`${r},${students[r].name},${students[r].course},${students[r].fees},${students[r].results}\n`;
 }
 let a=document.createElement('a');
 a.href='data:text/csv,'+encodeURIComponent(csv);
 a.download='students.csv';
 a.click();
}

function openPage(id){
 ['admission','semester','results','fees','gate','exam','idcard','changepass'].forEach(x=>document.getElementById(x).style.display='none');
 document.getElementById(id).style.display='block';
}

function changePass(){
 students[current].pass=newpass.value;
 save();
 alert("Password updated");
}

function logout(){location.reload();}
</script>
