# Particle-swarm-optimization-PSO-
Simple problem solving based on particle swarm optimization
题目：
------
![题目](https://user-images.githubusercontent.com/74950715/113649322-cfda2780-96c0-11eb-80fa-6f54ced10b11.jpg)
代码:
------
```matlab
clear all;clc
%%
%基本参数定义
N=1000;%粒子数目
C1=1.5;%学习因子1
C2=1.5;%学习因子2
W=0.7;%惯性权重
G=100;%最大迭代次数
D=30;%维度个数
V_max=1;
V_min=0.0001;
%%
%初始化
v=rand(D,N)*1;
x=randn(D,N)*10;
p_mybest=nan(D,N);
p_webest=nan(D,1);
%%
%进化
member=nan(1,G);
for i=1:G
    [v,x,p_mybest,p_webest]=newv(v,x,C1,C2,N,W,D,p_mybest,p_webest,V_max,V_min);
    member(1,i)=fh(p_webest,D);
end
%%
%画图
t=1:G;
plot(t,member,'g','LineWidth',1);
hold on;
plot(t,member,':r','LineWidth',3);
title('最小值曲线');
xlabel('代数');
ylabel('最小值')
hold off;
%%
%适应度函数
%输入为(30*1)矩阵,返回适应度函数
function y=fh(x,D)
    %x为（30，1）；
    temp_sum=x'*x;%累加x^2
    temp_mul=1;
    for i=1:D
        temp_mul=temp_mul*(cos(x(i)/sqrt(i)));
    end
    y=(1/4000)*temp_sum-temp_mul+1;
end
%%
%更新函数
%输入为（30*100）矩阵，返回更新速度
function [v,x,p_mybest,p_webest]=newv(v,x,C1,C2,N,W,D,p_mybest,p_webest,V_max,V_min)
   c1=C1*rand(D,N);
   c2=C2*rand(D,N);
   temp_x=x;
   x=x+v;
   if numel(find(isnan(p_webest)))==0
        v=W.*v+c1.*(p_mybest-temp_x)+c2.*(p_webest-temp_x);
       for i=1:D
           for j=1:N
               if v(i,j)>V_max
                   v(i,j)=V_max;
               end
               if v(i,j)<V_min
                   v(i,j)=V_min;
               end
           end
       end
   else
       v=W.*v;
       for i=1:D
           for j=1:N
               if v(i,j)>V_max
                   v(i,j)=V_max;
               end
               if v(i,j)<V_min
                   v(i,j)=V_min;
               end
           end
       end
   end
   p_mybest=mybest(x,N,p_mybest,D);%准备调用
   p_webest=webest(N,p_webest,p_mybest,D);%准备调用,顺序一会得换下
end
%%
%最好位置函数
function p_mybest=mybest(x,N,p_mybest,D)
    ynow=nan(1,N);
    for col=1:N
        ynow(1,col)=fh(x(:,col),D);
        if numel(find(isnan(p_mybest)))~=0
            p_mybest(:,col)=x(:,col);
        else
            if ynow(1,col)<=fh(p_mybest(:,col),D)
                p_mybest(:,col)=x(:,col);
            end
        end
    end
end

function p_webest=webest(N,p_webest,p_mybest,D)
    if numel(find(isnan(p_webest)))~=0
        p_webest=p_mybest(:,1);
    end
    fmin=fh(p_webest,D);
    for i=2:N
        fhp=fh(p_mybest(:,i),D);
        if fmin>fhp
            p_webest=p_mybest(:,i);
            fmin=fh(p_mybest(:,i),D);
        end
    end
    p_webest=p_webest;
end
%%
```
