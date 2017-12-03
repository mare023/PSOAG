%%----------ACKLEY FUN 1-----------%%

function y = fun1(x)
  n = 2;
  sum1 = 0;
  sum2 = 0;
  
  for i=1:n
   sum1 = sum1+x(i)^2;
   sum2 = sum2+cos((2*pi)*x(i));
  end
  
  y = 20 + exp(1)-20*exp(-0.2*sqrt(1/n*sum1))-exp(1/n*sum2);
  
end

%%-------------- T E S T -----------------%%

% %skiciranje ackley f-je za dve p-ljive
 %%u intervalu [10,10]
 [x1,x2] = meshgrid(-10:.1:10);
 [m,n]=size(x1);
 y=zeros(m,n);
 for i=1:m
     for j=1:n
         y(i,j)=fun1([x1(i,j) x2(i,j)]);
     end
 ends
 
 mesh(x1,x2,y);
 %%%

%===================== PSO TEST ===================%
 [x,fmin]=pso(@fun1,2);
 options=pso('options');
 options.tol=1e-8;
 [x,fmin]=pso(@fun1,2,options)


% =====================GA TEST =====================%
[x,fmin]=ga(fun1,nvars,A,b,Aeq,beq,LB,UB,nonlcons,options);
options=optimoptions('ga');
options.FunctionTolerance=1e-8;
Aeq=[]; %vrsta koliko i lin. ogr. jednakosti, a kolona koliko i promenljivih
beq=[]; %ono sa desne strane lin. ogr. jednakosti
A=[];
b=[];
[x,fmin]=ga(@fun1,2,A,b,Aeq,beq,[],[],[],options);
[x,fmin]=ga(@fun1,2)

%2.3====================ZADATAK SA OGRANICENJIMA ==========================%

%2.3
%------------- PSO FUNKCIJA
function y = F1(x)
    c=1e20; %neki veliki brojs
    y=fun1(x)+c*(x(1).^2+x(2).^2-25).^2+c*(-x(1)+x(2)-2).^2;

end
%%----------------- SKRIPTA 
[x,fmin]=pso(@F1,2,options)

%%-----------------------------------------------------------------------------------------%%
% GA  --------- FUNKCIJA OGRANICENJA -----------
function [ c,ceq] = ogranicenja( x )
    % ona se ovde stavljaju nelinearna ogranicenja
    % sve u c treba da bude <= 0
    % sve u ceq treba da bude = 0
    c=[];
    c1 = x(1).^2 + x(2).^2 - 25;
    ceq=[c1];  
end
---------------SKRIPTA--------------
% Aeq=[-1,1]; %koef. uz p-ljive u lin. ogr. tipa jednakosti
% beq=[2]; %ono sa desne strane lin. ogr. tipa jednakosti
% A=[]; %koef. uz p-ljive u ogranicenjima tipa nejednakosti
% b=[]; %ono sa desne strane ogranicenja tipa nejednakosti
% [x,fmin]=ga(@fun1,2,A,b,Aeq,beq,-10,10,@ogranicenja,options)
