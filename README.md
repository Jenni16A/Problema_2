# Problema_2
%Jennifer Lizeth Andrés Jorge
%Ángel Gabriel Elías Díaz

% Problem 2
% Implement Brent's Method

function z=Brent(f,a,b,tol)
	a=a; % lower limit
	b=b; % upper limit
	fa=feval(f,a);
	fb=feval(f,b);
% checks if there is a root in the interval
	if  ((fa > 0 & fb > 0) || (fa < 0 & fb < 0))
		z=NaN
		disp ('the  root is not in this interval')
		break
	end

	i=1;
	n=1000;
	c=b; % the value of b is saved in an auxiliary variable
	fc=fb;
	for i=1:n
% If there isn't root in the interval [c,b], then do change c=a and fc=fa and in another variable "d" keep the difference b-a
		if ((fb>0 & fc>0) || (fb<0 & fc<0))
			c=a;
			fc=fa;
			d=b-a;
			e=d;
		end
% Check if the image of f in b is greater than image of f in c, to shorten the interval [a, b]
		if  abs(fc)<abs(fb)
			a=b;
			b=c;
			c=a;
			fa=fb;
			fb=fc;
			fc=fa;
		end
		ep=3*(10^-8);
		tol1=2*ep*abs(b)+0.5*tol;
		xm=0.5*(c-b);
		if  (abs(xm)<=tol1 || fb==0)
			z=b;
			break
% The root was found, then ends the execution.
		end
		if  abs(e)>=tol1 & abs(fa)>abs(fb)
			s=fb/fa; % Quadratic Interpolation 
			if  a==c
				p=2*xm*s;
				q=1-s;
			else
				q=fa/fc;
				r=fb/fc;
				p=s*(2*xm*q*(q-r)-(b-a)*(r-1));
				q=(q-1)*(r-1)*(s-1);
			end 
			if  p>=0
				q=-q;
			end
			p=abs(p);
			if (2*p < min(3*xm*q-abs(tol1*q),abs(e*q)))
				e=d;  % interpolation is accepted
				d=p/q;
			else
				d=xm;% Interpolation failed
				e=d;
			end
		else
			d=xm;
			e=d;
		end
		a=b;   % Move the best last approach to a
		fa=fb;
		if (abs(d)>tol1)
			b=b+d;
		else
			b=b+(sign(xm)*tol1);
		end
		fb=feval(f,b);
	end
	z=b % final approach
end
