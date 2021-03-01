#include<iostream.h>
#include<process.h>
#include<conio.h>
#include<fstream.h>
#include<string.h>
#include<stdio.h>
#include<stdlib.h>
#include<graphics.h>
#include<dos.h>
#include<ctype.h>
#include<iomanip.h>
fstream fpm,fp2,fp3,fpc,fpr;
char pid[10],pname[20],cost[10];
int recount=0,k;
class manager
{
 private:
  char id[10],password[10];
 public:
  int authenticate(char[],char[]);
  void pack(char[]);
  void unpack(char[]);
  void insert();
  int search(char[]);
  void index_store();
  void unpack_index_file(char[],int);
  void del_product();
  void write_to_index();
  void display();
  void record_count();
  void modify();
};
class customer{

	public:int bill(char[]);
	void items_pack(char[],int);
	void pack_customer(char[],char[],int);
};


struct index
{
 char key[15];
 int offset;
};
index in[100];
manager m;
customer c;
int manager::authenticate(char id1[],char pass[])
{
 char buffer[50];
 char *id,*password;
 int i;
 fpm.open("manager.txt",ios::in);
 for(i=1;i<=10;i++)
 {
  fpm>>buffer;
  id=strtok(buffer,"|");
  password=strtok(NULL,'\0');
  if((strcmp(id,id1)==0)&&(strcmp(pass,password)==0))
  {
   cout<<"Authorization Successful\n";
   return  1;
  }
 }
 fpm.close();
 return 0;
}
void manager::modify()
{       char buffer[50];
	del_product();
	cout<<"Enter new values:\n";
	insert();
}



void manager::pack(char buffer[])
{
 cout<<"Product id,name and cost:\n";
 cin>>pid>>pname>>cost;
 strcpy(buffer,pid);
 strcat(buffer,"|");
 strcat(buffer,pname);
 strcat(buffer,"|");
 strcat(buffer,cost);
}

void manager::insert()
{
 char buffer[50],tem[50];
 char *pid;
 int i,j,pos;
 fp2.open("product.txt",ios::out|ios::ate);
 pack(buffer);
 strcpy(tem,buffer);
 pid=strtok(tem,"|");
 pos=search(pid);
 if(pos!=0)
 {
  cout<<"Customer already exists\n"<<endl;
  fp2.close();
  return;
 }
 recount++;
 cout<<"count="<<recount<<"\n";
 strcpy(in[recount].key,pid);
 cout<<in[recount].key<<"\n";
 in[recount].offset=fp2.tellg();
 cout<<in[recount].offset<<"\n";
 cout<<fp2.tellg()<<"\n";
 cout<<buffer<<"\n";
 fp2<<buffer;
 fp2<<"\n";
 index temp;

 for(i=1;i<recount;i++)
 {
  for(j=1;j<recount;j++)
  {
   if(strcmp(in[j].key,in[j+1].key)>0)
   {
    strcpy(temp.key,in[j].key);
    temp.offset=in[j].offset;
    cout<<in[j].key<<in[j+1].key<<"\n";

    strcpy(in[j].key,in[j+1].key);
    in[j].offset=in[j+1].offset;

    strcpy(in[j+1].key,temp.key);
    in[j+1].offset=temp.offset;
   }
  }
 }
 fp2.close();
}

int manager::search(char id[])
{
 int low=0,high=recount,mid;
 while(low<=high)
 {
  mid=(low+high)/2;
  if(strcmp(in[mid].key,id)==0)
	return mid;
  if(strcmp(in[mid].key,id)>0)
	high=mid-1;
  else
	low=mid+1;
 }
 return 0;
}

void manager::unpack(char buffer[])
{
 char *t;
 t=strtok(buffer,"|");
 gotoxy(3,k);
 cout<<t;
 gotoxy(20,k);
 t=strtok(NULL,"|");
 cout<<t;
 gotoxy(40,k);
 t=strtok(NULL,"\0");
 cout<<t;
 k++;
}

void manager::index_store()
{
 char buffer[50];
 fp3.open("index5.txt",ios::in);
 if(recount==0){
	return;
 }
 for(int i=1;i<=recount;i++)
 {
  fp3>>buffer;
  unpack_index_file(buffer,i);
 }
 fp3.close();
}

void manager::unpack_index_file(char buffer[],int i)
{
 char *t;
 t=strtok(buffer,"|");
 strcpy(in[i].key,t);
 t=strtok(NULL,'\0');
 in[i].offset=strtol(t,&t,0);

}

void manager::del_product()
{
 char name[10],buffer[50],c;
 int pos,i,r;
 fp2.open("product.txt",ios::out|ios::ate);
 cout<<"\nEnter the product id:\n";
 cin>>name;
 pos=search(name);
 //cout<<"Position:"<<pos;
 fp2.seekg(in[pos].offset,ios::beg);
 fp2<<"*";
 fp2.close();
 for(r=pos;r<recount;r++)
 {
  strcpy(in[r].key,in[r+1].key);
  in[r].offset=in[r+1].offset;
 }
 recount--;
}

void manager::write_to_index()
{
 fp3.open("index5.txt",ios::out|ios::trunc);
 for(int i=1;i<=recount;i++)
 {
  //cout<<"key:"<<in[i].key<<"\n"<<"offset:"<<in[i].offset<<"\n";
  fp3<<in[i].key<<"|"<<in[i].offset<<"\n";
 }
 fp3.close();
}

void manager::display()
{
 int i;
 char buffer[50];
 fp2.open("product.txt",ios::in);
 if(recount==0)
 {
  cout<<"No records to display\n";
  fp2.close();
  return;
 }
 /*cout<<"---------------------------\n";
 cout<<"-----PRIMARY INDEX---------\n";
 cout<<"---------------------------\n";
 cout<<"KEY\t\tOFFSET\n";
 cout<<"---------------------------\n";
 for(i=1;i<=recount;i++)
 {
  cout<<in[i].key<<"\t\t"<<in[i].offset<<endl;
 }
 cout<<"File contents are\n";
 cout<<"id\t\tname\t\tcost\n";*/
 for(i=1;i<=recount;i++)
 {
  fp2.seekg(in[i].offset,ios::beg);
  fp2>>buffer;
  unpack(buffer);
  cout<<endl;
 }
 fp2.close();
}

void manager::record_count()
{
 int j;
 char buffer[50];
 fp3.open("index5.txt",ios::in);
 j=0;
 while(strcmp(buffer,'\0')!=0)
 {
  fp3>>buffer;
  j++;
 }
 if(j!=0){
	recount=j-1;
 }
 fp3.close();
 index_store();
}
void customer::items_pack(char id[],int quant)
{       int pos;
	char buffer[50],temp[50],buffer1[50],*name,*cost;
	fpc.open("bill.txt",ios::out|ios::ate);
	pos=m.search(id);
	fp2.open("product.txt",ios::in);
	fp2.seekg(in[pos].offset,ios::beg);
	fp2>>buffer;
	strcpy(temp,buffer);
	name=strtok(temp,"|");
	name=strtok(NULL,"|");
	cost=strtok(NULL,'\0');
	fpc<<name<<"|"<<cost<<"|"<<quant<<"\n";
	fpc.close();
	fp2.close();
}
int customer::bill(char name[])
{       char buffer[50],temp[50],*t;

	long l,k1;
	int j,count,total=0,k=10;
	fpc.open("bill.txt",ios::in);
	j=0;
	while(strcmp(buffer,'\0')!=0)
	{
		 fpc>>buffer;
		 j++;
	}
	if(j!=0){
		count=j-1;
	}
	fpc.close();
	clrscr();
	fpc.open("bill.txt",ios::in);

			clearviewport();
			rectangle(0,0,600,350);
			settextstyle(BOLD_FONT,HORIZ_DIR,6);
			setcolor(LIGHTBLUE);
			outtextxy(5,0,"P");
			outtextxy(25,4,"P");
			settextstyle(TRIPLEX_FONT,HORIZ_DIR,3);
			setcolor(RED+DARKGRAY);
			outtextxy(230,5,"PICK & PAY");
			settextstyle(TRIPLEX_FONT,HORIZ_DIR,1);
			outtextxy(260,30,"Bangalore");
			setcolor(LIGHTBLUE);
			outtextxy(450,10,"Customer:");
			outtextxy(500,30,name);
			settextstyle(TRIPLEX_FONT,HORIZ_DIR,3);
			setcolor(RED+DARKGRAY);
			outtextxy(270,58,"BILL");
			line(0,110,600,110);
			line(0,130,600,130);
			gotoxy(3,8);
			cout<<"SI NO";
			gotoxy(20,8);
			cout<<"NAME";
			gotoxy(40,8);
			cout<<"COST";
			gotoxy(52,8);
			cout<<"QUANTITY";
			gotoxy(67,8);
			cout<<"TOTAL";
			line(60,110,60,350);
			line(220,110,220,350);
			line(390,110,390,350);
			line(490,110,490,350);

	for(j=1;j<=count;j++)
	{       //i++;
		fpc>>buffer;
		strcpy(temp,buffer);
		gotoxy(3,k);
		 cout<<j;
		 t=strtok(buffer,"|");
		 gotoxy(20,k);
		 cout<<t;
		 t=strtok(NULL,"|");
		 k1=strtol(t,&t,0);
		 gotoxy(40,k);
		 cout<<k1;
		 t=strtok(NULL,'\0');
		 l=strtol(t,&t,0);
		 gotoxy(52,k);
		 cout<<l;
		 gotoxy(67,k);
		 cout<<k1*l;
		 k++;
		 //line(0,k*10,600,k*10);
		 total=total+(k1*l);
	}

	gotoxy(52,23);
	cout<<"TOTAL:";
	gotoxy(67,23);
	cout<<total;
	gotoxy(52,24);
	cout<<"GST:";
	gotoxy(67,24);

	cout<<(total*0.05);
	gotoxy(52,25);
	cout<<"GRAND TOTAL";
	 gotoxy(67,25);
	 cout<<(total+(total*0.05));

	fpc.close();
	getch();
	remove("bill.txt");
	return total;
}
void customer::pack_customer(char name[],char phone[],int bill)
{
	fpr.open("customers.txt",ios::out|ios::ate);
	fpr<<name<<"|"<<phone<<"|"<<bill<<"\n";
	fpr.close();
}
void welcome()
{

//textcolor(WHITE+LIGHTGRAY);
	gotoxy(20,9);
	cout<<"GROCERY STORE & BILL GENERATION SYSTEM";
	gotoxy(20,12);
	cout<<"PROJECT   ";
	gotoxy(20,14);
	cout<<"DEVELOPED BY:  ";
	gotoxy(20,16);
	setcolor(GREEN);
	cout<<"PRIYANKA V RAO";
	gotoxy(20,17);
	cout<<"PRERANA H";
  gotoxy(20,24);
  cout<<"PRESS ANY KEY TO CONTINUE";
  getch();
}




void main()
{
 int i, j = 1, gd = DETECT, gm;
 char ci[10];
 int quantity,bill;
 char buffer[50];
 char name[20],phone[10];
 char z[10],g,x;
 int arf[10],arv[10],arp[10],arh[10],ard[10],arb[10],ars[10],arc[10];
 int pcount=0,fcount=0,vcount=0,hcount=0,dcount=0,bcount=0,scount=0,ccount=0,c1=1,c2=1,c3=1,c4=1,c5=1,c6=1,c7=1,c8=1;
 int ch,value,choice,ch1,cf,h,d,b;
 char id[10],password[20];
 //clrscr();
 initgraph(&gd,&gm,"C:\\TURBOC3\\BGI");
 welcome();
 clrscr();
  // setviewport(0,0,639,440,1);
     for( i = 0 ; i <= 420 ; i = i + 10 )
   {
	 setcolor(WHITE);
	 setfillstyle(SOLID_FILL,WHITE);
	 fillellipse(320,90,80,25);


   setcolor(YELLOW);
   ellipse(320,90,0,360,75,20);
   setcolor(WHITE);
   rectangle(210,80,430,60);
   rectangle(190,100,450,80);
   rectangle(220,130,280,210);//inner left most
   rectangle(285,210,345,130);//inner 2
   rectangle(360,210,420,130);//inner right most
   rectangle(370,160,375,180);//handle
   rectangle(210,220,430,100);//upper1
   rectangle(190,220,450,230);//base
   setcolor(j);
   settextstyle(DEFAULT_FONT,HORIZ_DIR,2);
   outtextxy(265,85,"GROCERY");

   settextstyle(BOLD_FONT,HORIZ_DIR,4);
   outtextxy(190,240,"PICK & PAY");

      line(50+i,400,55+i,390);//slant line
      line(55+i,390,20+i,300);//left slant line
      line(55+i,390,190+i,370);//upper straight line
      line(190+i,370,190+i,340);//vertical line
      line(10+i,300,20+i,300);//handle
      line(35+i,340,190+i,340);//hl
      line(40+i,350,190+i,350);//hl
      line(45+i,360,190+i,360); //hl
      line(50+i,370,190+i,370);//hl
      line(50+i,370,50+i,340);//vl
      line(90+i,370,90+i,340);
      line(70+i,370,70+i,340);
      line(110+i,370,110+i,340);
      line(130+i,370,130+i,340);
      line(150+i,370,150+i,340);
      line(170+i,370,170+i,340);
      line(50+i,400,200+i,400); //base line
      //j++;
      setcolor(j);
      circle(75+i,410,10);
      setfillstyle(SOLID_FILL,j);
      floodfill(75+i,410,j);

      setcolor(0);
      circle(75+i,410,5);
       setfillstyle(SOLID_FILL,0);
      floodfill(75+i,410,0);

      setcolor(j);
      circle(175+i,410,10);
      setfillstyle(SOLID_FILL,j);
      floodfill(175+i,410,j);

      setcolor(0);
      circle(175+i,410,5);
      setfillstyle(SOLID_FILL,0);
      floodfill(175+i,410,0);
      j++;
      setcolor(j);
      delay(50);


      if( i == 420 )
	 break;

    clearviewport();


   }

   clearviewport();
  // closegraph();
 m.record_count();
 setcolor(1);
 settextstyle(TRIPLEX_FONT,HORIZ_DIR,5);

 outtextxy(70,70,"\nWELCOME TO PICK & PAY");
 setcolor(8);
 outtextxy(150,150,"\n 1.MANAGER ");
 outtextxy(150,200,"\n 2.CUSTOMER");
  settextstyle(TRIPLEX_FONT,HORIZ_DIR,4);
 outtextxy(120,360,"\n ENTER YOUR CHOICE(1,2):");

 gotoxy(75,24);
 cin>>ch;
clearviewport();
//clrscr();
 textcolor(BLACK);
 switch(ch)
 {
  case 1:setcolor(RED);
  settextstyle(TRIPLEX_FONT,HORIZ_DIR,3);
  outtextxy(180,240,"WELCOME TO PICK & PAY");
 // outtextxy(280,340,"& PAY");
  //textcolor(WHITE);
  setcolor(YELLOW);
  setbkcolor(BLACK);
  rectangle(180,40,450,130);
  settextstyle(TRIPLEX_FONT,HORIZ_DIR,3);
  setcolor(LIGHTBLUE);
  outtextxy(200,60,"Enter userid:");
  gotoxy(50,5);
  cin>>id;
  outtextxy(200,90,"Enter password:");
  gotoxy(51,7);
  cin>>password;
  value=m.authenticate(id,password);
  if(value==1)
  {
   for(;;)
   {
   clearviewport();
   clrscr();
   setcolor(WHITE);
settextstyle(TRIPLEX_FONT ,HORIZ_DIR,4);
outtextxy(100,10,"***********MENU************");
  setcolor(DARKGRAY);
   textcolor(BLACK);
settextstyle(TRIPLEX_FONT ,HORIZ_DIR,4);
outtextxy(220,50,"\n\n1.INSERT " );
outtextxy(220,100,"\n\n2.DISPLAY" );
outtextxy(220,150,"\n\n3.DELETE " );
outtextxy(220,200,"4.\n\nMODIFY " );
outtextxy(220,250,"\n\n\n5.EXIT" );
settextstyle(TRIPLEX_FONT,HORIZ_DIR,3);
outtextxy(210,350,"Enter the choice ->");
gotoxy(60,23);
cin>>choice;
   switch(choice)
   {
    case 1:clrscr();m.insert();m.write_to_index();break;
    case 2:m.write_to_index();clrscr();
	   clearviewport();

		rectangle(0,0,430,450);
			line(0,30,430,30);
			line(0,50,430,50);
			k=5;
			settextstyle(TRIPLEX_FONT ,HORIZ_DIR,2);
			setcolor(LIGHTBLUE);
			 outtextxy(100,1,"PRODUCT DETAILS");
			 textcolor(BLACK);
			 setcolor(DARKGRAY);
			gotoxy(3,3);
			cout<<"PRODUCT ID";
			gotoxy(20,3);
			cout<<"PRODUCT NAME";
			gotoxy(40,3);
			cout<<"  COST";
			line(140,50,140,450);
			line(270,50,270,450);
	   m.display();
	   getch();
	   clearviewport();break;
    case 3:clrscr();m.del_product();m.write_to_index();break;
    case 4:clrscr();m.modify();m.write_to_index();break;
    case 5:clrscr();settextstyle(TRIPLEX_FONT ,HORIZ_DIR,8);
			setcolor(LIGHTBLUE);
		outtextxy(100,100,"THANK YOU");
		getch();
		exit(0);
   }
  }
 }
 else if(value==0){
		clearviewport();
		setcolor(RED);
		settextstyle(TRIPLEX_FONT ,HORIZ_DIR,4);
		outtextxy(50,50,"Invalid username or password!!!!!!!");
		getch();
		exit(0);
		}
 break;
 case 2:m.index_store();
 //setcolor(YELLOW);
  //setbkcolor(BLACK);
  //rectangle(180,40,450,130);
  settextstyle(TRIPLEX_FONT,HORIZ_DIR,2);
  setcolor(GREEN);
  outtextxy(150,60,"Enter your name:");
  gotoxy(45,5);
  cin>>name;
  outtextxy(150,90,"Enter your Phone number:");
  gotoxy(55,7);
  cin>>phone;
 fp2.open("product.txt",ios::in);
 for(int t=1;t<=recount;t++)
 {
	g=in[t].key[0];
	if(g=='f')
	{       fcount++;
		arf[c1]=in[t].offset;
		c1++;
	}
	else if(g=='v')
	{       vcount++;
		arv[c2]=in[t].offset;
		c2++;
	}
	else if(g=='p')
	{       pcount++;
		arp[c3]=in[t].offset;
		c3++;
	}
	else if(g=='h')
	{       hcount++;
		arh[c4]=in[t].offset;
		c4++;
	}
	else if(g=='d')
	{       dcount++;
		ard[c5]=in[t].offset;
		c5++;
	}
	else if(g=='b')
	{       bcount++;
		arb[c6]=in[t].offset;
		c6++;
	}
	else if(g=='c')
	{       ccount++;
		arc[c7]=in[t].offset;
		c7++;
	}
	else if(g=='s')
	{       scount++;
		ars[c8]=in[t].offset;
		c8++;
	}

}
fp2.close();
 //clrscr();
 for(;;)
 {      clrscr();
	clearviewport();
	settextstyle(TRIPLEX_FONT ,HORIZ_DIR,6);
	outtextxy(180,60,"PICK & PAY");

	settextstyle(TRIPLEX_FONT ,HORIZ_DIR,3);
	setcolor(YELLOW);
	outtextxy(70,150,"1.FRUITS");
	outtextxy(350,150,"5.DAIRY PRODUCTS");
	outtextxy(70,170,"2.VEGETABLES");
	outtextxy(350,170,"6.BEVERAGES");
	outtextxy(70,190,"3.PERSONAL CARE");
	outtextxy(350,190,"7.CHOCOLATES");
	outtextxy(70,210,"4.HOME CARE");
	outtextxy(350,210,"8.SNACKS");
	settextstyle(TRIPLEX_FONT ,HORIZ_DIR,4);
	setcolor(RED+DARKGRAY);
	outtextxy(200,290,"9.GENERATE BILL");
	outtextxy(200,330,"10.EXIT   ");
	settextstyle(TRIPLEX_FONT,HORIZ_DIR,3);
	outtextxy(210,380,"Enter the choice->");
	gotoxy(56,25);
	cin>>ch1;
	switch(ch1)
	{
	case 1:
			fp2.open("product.txt",ios::in);
			clrscr();
			clearviewport();

			rectangle(0,0,430,250);
			line(0,50,430,50);
			line(0,90,430,90);
			k=7;
			settextstyle(TRIPLEX_FONT ,HORIZ_DIR,4);
			setcolor(RED+WHITE);
			 outtextxy(80,3,"******FRUITS*******");
			 textcolor(BLACK);
			gotoxy(3,5);
			cout<<"PRODUCT ID";
			gotoxy(20,5);
			cout<<"PRODUCT NAME";
			gotoxy(40,5);
			cout<<"COST(per kg)";
			line(140,90,140,250);
			line(270,90,270,250);
		for(h=1;h<=fcount;h++)
		{
			fp2.seekg(arf[h],ios::beg);
			fp2>>buffer;
			m.unpack(buffer);
		}
		fp2.close();
		x='n';
		while(x!='y')
		{       settextstyle(TRIPLEX_FONT ,HORIZ_DIR,2);
			outtextxy(10,270,"Enter the product id:");
			gotoxy(32,18);
			cin>>ci;
			outtextxy(10,300,"Enter quantity in(kg):");
			gotoxy(35,20);
			cin>>quantity;
			c.items_pack(ci,quantity);
			outtextxy(10,330,"Press y when done else press n:");
			gotoxy(46,22);
			cin>>x;

		}
		break;
	case 2:  fp2.open("product.txt",ios::in);
		clrscr();
			clearviewport();

			rectangle(0,0,430,250);
			line(0,50,430,50);
			line(0,90,430,90);
			k=7;
			settextstyle(TRIPLEX_FONT ,HORIZ_DIR,4);
			setcolor(RED+WHITE);
			 outtextxy(80,3,"****VEGETABLES****");
			 textcolor(BLACK);
			gotoxy(3,5);
			cout<<"PRODUCT ID";
			gotoxy(20,5);
			cout<<"PRODUCT NAME";
			gotoxy(40,5);
			cout<<"COST(per kg)";
			line(140,90,140,250);
			line(270,90,270,250);
		for(d=1;d<=vcount;d++)
		{      // cout<<arv[d]<<endl;
			fp2.seekg(arv[d],ios::beg);
			fp2>>buffer;
			m.unpack(buffer);
		}
		fp2.close();
		x='n';
		while(x!='y')
		{

			settextstyle(TRIPLEX_FONT ,HORIZ_DIR,2);
			outtextxy(10,270,"Enter the product id:");
			gotoxy(32,18);
			cin>>ci;
			outtextxy(10,300,"Enter quantity in(kg):");
			gotoxy(35,20);
			cin>>quantity;
			c.items_pack(ci,quantity);
			outtextxy(10,330,"Press y when done else press n:");
			gotoxy(46,22);
			cin>>x;
		}
		break;
	case 3:  fp2.open("product.txt",ios::in);
		clrscr();
			clearviewport();

			rectangle(0,0,430,250);
			line(0,50,430,50);
			line(0,90,430,90);
			k=7;
			settextstyle(TRIPLEX_FONT ,HORIZ_DIR,4);
			setcolor(RED+WHITE);
			 outtextxy(80,3,"**PERSONAL CARE**");
			 textcolor(BLACK);
			gotoxy(3,5);
			cout<<"PRODUCT ID";
			gotoxy(20,5);
			cout<<"PRODUCT NAME";
			gotoxy(40,5);
			cout<<"COST(per unit)";
			line(140,90,140,250);
			line(270,90,270,250);
		for(d=1;d<=pcount;d++)
		{      // cout<<arp[d]<<endl;
			fp2.seekg(arp[d],ios::beg);
			fp2>>buffer;
			m.unpack(buffer);
		}
		fp2.close();
		x='n';
		while(x!='y')
		{

			settextstyle(TRIPLEX_FONT ,HORIZ_DIR,2);
			outtextxy(10,270,"Enter the product id:");
			gotoxy(32,18);
			cin>>ci;
			outtextxy(10,300,"Enter quantity in(units):");
			gotoxy(35,20);
			cin>>quantity;
			c.items_pack(ci,quantity);
			outtextxy(10,330,"Press y when done else press n:");
			gotoxy(46,22);
			cin>>x;
		}
		break;
	case 4:  fp2.open("product.txt",ios::in);
		clrscr();
			clearviewport();

			rectangle(0,0,430,250);
			line(0,50,430,50);
			line(0,90,430,90);
			k=7;
			settextstyle(TRIPLEX_FONT ,HORIZ_DIR,4);
			setcolor(RED+WHITE);
			 outtextxy(80,3,"****HOME CARE****");
			 textcolor(BLACK);
			gotoxy(3,5);
			cout<<"PRODUCT ID";
			gotoxy(20,5);
			cout<<"PRODUCT NAME";
			gotoxy(40,5);
			cout<<"COST(per unit)";
			line(140,90,140,250);
			line(270,90,270,250);
		for(d=1;d<=hcount;d++)
		{
			fp2.seekg(arh[d],ios::beg);
			fp2>>buffer;
			m.unpack(buffer);
		}
		fp2.close();
		x='n';
		while(x!='y')
		{

			settextstyle(TRIPLEX_FONT ,HORIZ_DIR,2);
			outtextxy(10,270,"Enter the product id:");
			gotoxy(32,18);
			cin>>ci;
			outtextxy(10,300,"Enter quantity in(units):");
			gotoxy(35,20);
			cin>>quantity;
			c.items_pack(ci,quantity);
			outtextxy(10,330,"Press y when done else press n:");
			gotoxy(46,22);
			cin>>x;
		}
		break;
	case 5:  fp2.open("product.txt",ios::in);
		clrscr();
			clearviewport();

			rectangle(0,0,430,250);
			line(0,50,430,50);
			line(0,90,430,90);
			k=7;
			settextstyle(TRIPLEX_FONT ,HORIZ_DIR,4);
			setcolor(RED+WHITE);
			 outtextxy(80,3,"DAIRY PRODUCTS");
			 textcolor(BLACK);
			gotoxy(3,5);
			cout<<"PRODUCT ID";
			gotoxy(20,5);
			cout<<"PRODUCT NAME";
			gotoxy(40,5);
			cout<<"COST(per unit)";
			line(140,90,140,250);
			line(270,90,270,250);
		for(d=1;d<=dcount;d++)
		{      // cout<<arv[d]<<endl;
			fp2.seekg(ard[d],ios::beg);
			fp2>>buffer;
			m.unpack(buffer);
		}
		fp2.close();
		x='n';
		while(x!='y')
		{

			settextstyle(TRIPLEX_FONT ,HORIZ_DIR,2);
			outtextxy(10,270,"Enter the product id:");
			gotoxy(32,18);
			cin>>ci;
			outtextxy(10,300,"Enter quantity in(units):");
			gotoxy(35,20);
			cin>>quantity;
			c.items_pack(ci,quantity);
			outtextxy(10,330,"Press y when done else press n:");
			gotoxy(46,22);
			cin>>x;
		}
		break;
	case 6:  fp2.open("product.txt",ios::in);
		clrscr();
			clearviewport();

			rectangle(0,0,430,250);
			line(0,50,430,50);
			line(0,90,430,90);
			k=7;
			settextstyle(TRIPLEX_FONT ,HORIZ_DIR,4);
			setcolor(RED+WHITE);
			 outtextxy(80,3,"****BEVERAGES****");
			 textcolor(BLACK);
			gotoxy(3,5);
			cout<<"PRODUCT ID";
			gotoxy(20,5);
			cout<<"PRODUCT NAME";
			gotoxy(40,5);
			cout<<"COST(per kg)";
			line(140,90,140,250);
			line(270,90,270,250);
		for(d=1;d<=bcount;d++)
		{      // cout<<arb[d]<<endl;
			fp2.seekg(arb[d],ios::beg);
			fp2>>buffer;
			m.unpack(buffer);
		}
		fp2.close();
		x='n';
		while(x!='y')
		{

			settextstyle(TRIPLEX_FONT ,HORIZ_DIR,2);
			outtextxy(10,270,"Enter the product id:");
			gotoxy(32,18);
			cin>>ci;
			outtextxy(10,300,"Enter quantity in(kg):");
			gotoxy(35,20);
			cin>>quantity;
			c.items_pack(ci,quantity);
			outtextxy(10,330,"Press y when done else press n:");
			gotoxy(46,22);
			cin>>x;
		}
		break;
	case 7:  fp2.open("product.txt",ios::in);
		clrscr();
			clearviewport();

			rectangle(0,0,430,250);
			line(0,50,430,50);
			line(0,90,430,90);
			k=7;
			settextstyle(TRIPLEX_FONT ,HORIZ_DIR,4);
			setcolor(RED+WHITE);
			 outtextxy(80,3,"****CHOCOLATES****");
			 textcolor(BLACK);
			gotoxy(3,5);
			cout<<"PRODUCT ID";
			gotoxy(20,5);
			cout<<"PRODUCT NAME";
			gotoxy(40,5);
			cout<<"COST(per unit)";
			line(140,90,140,250);
			line(270,90,270,250);
		for(d=1;d<=ccount;d++)
		{      // cout<<arc[d]<<endl;
			fp2.seekg(arc[d],ios::beg);
			fp2>>buffer;
			m.unpack(buffer);
		}
		fp2.close();
		x='n';
		while(x!='y')
		{

			settextstyle(TRIPLEX_FONT ,HORIZ_DIR,2);
			outtextxy(10,270,"Enter the product id:");
			gotoxy(32,18);
			cin>>ci;
			outtextxy(10,300,"Enter quantity in(units):");
			gotoxy(35,20);
			cin>>quantity;
			c.items_pack(ci,quantity);
			outtextxy(10,330,"Press y when done else press n:");
			gotoxy(46,22);
			cin>>x;
		}
		break;
	case 8:  fp2.open("product.txt",ios::in);
		clrscr();
			clearviewport();

			rectangle(0,0,430,250);
			line(0,50,430,50);
			line(0,90,430,90);
			k=7;
			settextstyle(TRIPLEX_FONT ,HORIZ_DIR,4);
			setcolor(RED+WHITE);
			 outtextxy(80,3,"******SNACKS******");
			 textcolor(BLACK);
			gotoxy(3,5);
			cout<<"PRODUCT ID";
			gotoxy(20,5);
			cout<<"PRODUCT NAME";
			gotoxy(40,5);
			cout<<"COST(per unit)";
			line(140,90,140,250);
			line(270,90,270,250);
		for(d=1;d<=scount;d++)
		{      // cout<<ars[d]<<endl;
			fp2.seekg(ars[d],ios::beg);
			fp2>>buffer;
			m.unpack(buffer);
		}
		fp2.close();
		x='n';
		while(x!='y')
		{

			settextstyle(TRIPLEX_FONT ,HORIZ_DIR,2);
			outtextxy(10,270,"Enter the product id:");
			gotoxy(32,18);
			cin>>ci;
			outtextxy(10,300,"Enter quantity in(units):");
			gotoxy(35,20);
			cin>>quantity;
			c.items_pack(ci,quantity);
			outtextxy(10,330,"Press y when done else press n:");
			gotoxy(46,22);
			cin>>x;
		}
		break;
	case 9://clrscr();clearviewport();
		bill=c.bill(name);
		k=10;
		c.pack_customer(name,phone,bill);

		break;
	case 10:settextstyle(TRIPLEX_FONT ,HORIZ_DIR,8);
			setcolor(LIGHTBLUE);outtextxy(100,100,"THANK YOU");
		getch();
		exit(0);


	 }
 }
	 break;

 default:settextstyle(TRIPLEX_FONT ,HORIZ_DIR,8);
			setcolor(LIGHTBLUE);outtextxy(100,100,"THANK YOU");
	getch();
	exit(0);
}
closegraph();


getch();
}
