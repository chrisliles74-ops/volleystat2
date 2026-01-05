let p=[
{id:1,n:3,name:"Player 1",k:0,e:0,ta:0,d:0,b:0,ast:0,sr1:0,sr2:0,sr3:0},
{id:2,n:7,name:"Player 2",k:0,e:0,ta:0,d:0,b:0,ast:0,sr1:0,sr2:0,sr3:0},
{id:3,n:12,name:"Player 3",k:0,e:0,ta:0,d:0,b:0,ast:0,sr1:0,sr2:0,sr3:0},
{id:4,n:18,name:"Player 4",k:0,e:0,ta:0,d:0,b:0,ast:0,sr1:0,sr2:0,sr3:0,lib:true},
{id:5,n:21,name:"Player 5",k:0,e:0,ta:0,d:0,b:0,ast:0,sr1:0,sr2:0,sr3:0},
{id:6,n:25,name:"Player 6",k:0,e:0,ta:0,d:0,b:0,ast:0,sr1:0,sr2:0,sr3:0},
{id:7,n:32,name:"Player 7",k:0,e:0,ta:0,d:0,b:0,ast:0,sr1:0,sr2:0,sr3:0},
{id:8,n:41,name:"Player 8",k:0,e:0,ta:0,d:0,b:0,ast:0,sr1:0,sr2:0,sr3:0},
{id:9,n:44,name:"Player 9",k:0,e:0,ta:0,d:0,b:0,ast:0,sr1:0,sr2:0,sr3:0},
{id:10,n:52,name:"Player 10",k:0,e:0,ta:0,d:0,b:0,ast:0,sr1:0,sr2:0,sr3:0},
{id:11,n:99,name:"Player 11",k:0,e:0,ta:0,d:0,b:0,ast:0,sr1:0,sr2:0,sr3:0}
];# volleystat2
`<button class="player 
${sel==x.id?'active':''} 
${x.lib?'libero':''} 
${p.indexOf(x) < 6 ? 'oncourt' : 'bench'}"
onclick="sel=${x.id};draw()">
#${x.n} ${x.name} ${x.lib?'(L)':''}
</button>`
.oncourt { border:3px solid #2196f3; }
.bench { opacity:0.4; }
.libero { background:#ffd54f !important; }
<div>
<button onclick="stat('k')">Kill</button>
<button onclick="stat('e')">Error</button>
<button onclick="stat('d')">Dig</button>
<button onclick="stat('b')">Block</button>
<button onclick="stat('ast')">Assist</button>
</div>

<div>
<button onclick="stat('sr1')">SR 1</button>
<button onclick="stat('sr2')">SR 2</button>
<button onclick="stat('sr3')">SR 3</button>
</div>
function stat(t){
if(!sel) return;
undoStack.push(JSON.stringify(p));
let x = p.find(z=>z.id==sel);

if(t=="k"){ x.k++; x.ta++; }
if(t=="e"){ x.e++; x.ta++; }
if(t=="d") x.d++;
if(t=="b") x.b++;
if(t=="ast") x.ast++;

if(t=="sr1") x.sr1++;
if(t=="sr2") x.sr2++;
if(t=="sr3") x.sr3++;
}
function pdf(){
const {jsPDF}=window.jspdf;
let d=new jsPDF({orientation:"landscape"});

// TEAM SR
let t1=0,t2=0,t3=0;
p.forEach(x=>{
  t1+=x.sr1; t2+=x.sr2; t3+=x.sr3;
});
let tTot=t1+t2+t3;
let teamSR=tTot?((t1*1+t2*2+t3*3)/tTot).toFixed(2):"–";

d.text(`TEAM SR AVG: ${teamSR}`,10,15);

let y=30;
p.forEach(x=>{
  let hp=x.ta?((x.k-x.e)/x.ta).toFixed(3):".000";
  let srTot=x.sr1+x.sr2+x.sr3;
  let srAvg=srTot?((x.sr1*1+x.sr2*2+x.sr3*3)/srTot).toFixed(2):"–";

  d.text(
`#${x.n} ${x.name}  K:${x.k} E:${x.e} TA:${x.ta} H%:${hp}  SR:${srAvg}  D:${x.d} B:${x.b} A:${x.ast}`,
10,y
  );
  y+=10;
});

d.save("match.pdf");
}
