conn sys/coti as sysdba

create user sabado01 identified by coti
default tablespace users
quota 20m on users;

grant create session, create table, create sequence, create procedure, create trigger, create view to sabado01;

create table cliente (idCliente number(15) primary key,
nome varchar(50), email varchar(50) unique);


create sequence seq_cliente;

set serveroutput on size 10000;

--procedure ou function que insere ou atualiza valores utiliza com exec na frente function sem inclusão ou alteração somente select 
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

  --descricao (consulta) ( select , (insert ou update ou delete) )
 
--insert, delete, update

set serveroutput on size 10000;
create or replace function fncinclusao ( vnome in varchar2, vemail in
 varchar2)  return number 
  as
begin
 insert into cliente values (seq_cliente.nextval, vnome, vemail);
 commit;
 return 1;
 exception when others then  
 dbms_output.put_line('Error :' || sqlerrm);
 return -1;

end;
/

 var resposta number;
 exec  :resposta := fncinclusao('filhalu','filha@lu.com');
 print resposta;
-------------------------------------------------------------------------------------
--exemplo
 create or replace function calculo (vnum1 in number, vnum2 in number,
 voperacao in varchar)   return varchar2
as
  begin
    if  voperacao = 'soma' then
    return   'soma :'   ||  (vnum1 + vnum2) ;
    elsif  voperacao = 'subtracao' then 
   return   'subtracao :'   ||  (vnum1 - vnum2) ; 
    elsif  voperacao = 'multiplicacao' then 
   return 'multiplicacao :'   ||  (vnum1 * vnum2) ; 
    elsif  voperacao = 'divisao' then 
   return  'divisao  :'   ||  (vnum1 / vnum2) ; 
     else
   return  'nao encontrada a operacao'; 
     end if;

 exception when others then
  return  'error: ' || sqlerrm;
end;
/


   select  calculo (100, 200, 'soma') from dual; 
  select   calculo (400, 200, 'subtracao') from dual; 
 select   calculo (5, 5, 'multiplicacao') from dual; 
select    calculo (10, 2, 'divisao') from dual; 
select    calculo (10, 0, 'divisao') from dual; 
