conn sys/coti as sysdba

create user sabado01 identified by coti
default tablespace users
quota 20m on users;

grant create session, create table, create sequence, create procedure, create trigger, create view to sabado01;

create table cliente (idCliente number(15) primary key,
nome varchar(50), email varchar(50) unique);


create sequence seq_cliente;

set serveroutput on size 10000;

create or replace procedure inclusao(vnome in varchar2, vemail in varchar2, vresp out number)
as 

begin
	insert into cliente values (seq_cliente.nextval, vnome, vemail);
	commit;
	dbms_output.put_line('dados gravados cliente');
vresp := 1;


exception when others then
	vresp := -1;
dbms_output.put_line('Error Encontrado ...');

end;
/
var resp number;
exec inclusao ('juliana', 'juju@gmail.com', :resp);
print resp;

-----------------------------------------------------------------------------------

create or replace function fncinclusao(vnome in varchar2, vemail in varchar2) as
return number
begin
	insert into cliente values(seq_cliente.nextval, vnome, vemail);

exception when others then

end;
/
