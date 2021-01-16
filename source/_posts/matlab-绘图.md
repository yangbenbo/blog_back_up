---
title: matlab 绘图
date: 2019-10-24 09:52:50
categories:
- Software
tags:
- matlab

---
# 常用绘图命令
## 命令查看
查看所有画图相关命令

    help graph2d
    help graph3d
## 图形窗口
    figure(n)    % 打开窗口n
    subplot(m,n,p)   % m行n列窗口第p个
## 坐标轴
- axis([xmin xmax ymin ymax])

- axis equal 使x,y轴的单位长度相同

- axis square 出图为正方形

- axis off 清除坐标刻度

- semilogx,semilogy 绘制以x/y轴为对数坐标，以10为底，y/x轴为线性坐标的半对数坐标图形

- loglog 绘制全对数坐标图，即x,y轴全取对数   
## 文字标示
- text(x,y,‘字符串’) 在图形的指定坐标（x,y)处表示’字符串’中的内容
- gtext('说明文字’）利用鼠标在图形的某一位置标示说明文字。执行完绘图命令后再执行gtext('说明文字‘）命令
- title('字符串’）图形标题
- xlabel('字符串‘),ylabel(‘字符串’),zlabel('字符串’)，设置x,y,z轴的坐标轴名称。如需输入特殊文字，用\开头
- legend(‘字符串1’,‘字符串1’,‘字符串1’……）对图形上多条线按照绘图顺序进行说明 
## 栅格
- grid 给图形加栅格
- grid on 给坐标系加栅格
- grid off 删除当前坐标系的栅格
- grid minor  细化栅格
## 图形覆盖/保持
- hold on 当前图形保持，且下条图形仍然绘制在该张图形上
- hold off 新图覆盖旧图 
## 视角方向

- view(az,el)                   给三维空间图形设置观察点的方位角az与仰角el

- view([az,el])                 同上

- view([x,y,z])                 将点(x,y,z)设置为视点

- view(2)                       设置默认的二维形式视点，其中az = 0,el = 90,即从z轴上方观看

- **view(3)**                       设置默认的三维形式视点，其中az = -37.5, ell = 30

- view(T)                       根据转换矩阵T设置视点，其中T为4*4阶的矩阵，如同用命令viewmtx生成的透视转换矩阵一样（注意：在2014版本中，这条命令好像没了）

- [az,el]=view                  返回当前的方位角az与仰角el

- T = view                      返回当前的4*4阶的转换矩阵T

## 例子

例子1
   
    clc;clear;close all;    % 清除变量　输入　关闭绘图
    scrsz = get(groot,'ScreenSize');
    figure('Position',[1 scrsz(4)/2 scrsz(3)/2 scrsz(4)/2]);  % 图像框为屏幕左上角1/4　像素
    limAxes = [-1.2 0.8 -1.1 0.9];
    xx = round(linspace(1,64,nbSamples));   % round 4舍5入
    clrmap = colormap('jet');
    clrmap = min(clrmap(xx,:),.95);
    colPegs = [[.9,.5,.9];[.5,.9,.5]];

    subplot(1,2,1); hold on; box on; title('Demonstrations');
    
    plot(b(1), b(2),'.','markersize',30,'color',colPegs(1,:)-[.05,.05,.05]);
    plot(Data(1,:), Data(2,:),'-','linewidth',1.5,'color',clrmap(n,:));
    
    axis(limAxes); axis square; 
    set(gca,'xtick',[],'ytick',[]);  % 清除坐标轴刻度 axis off

例子2

    f = figure;
    hold on;box on;
    P1 = plot(x, y,'linewidth',1.2);
    P2 = plot(mean_force, 0,'o','linewidth',1.2);   % 均值
    P3 = plot(threhold, 0,'o','linewidth',1.2);   % 均值
    
    axis([0 28 0 0.14]);
    
    width = 240;   % 240pt  US letter 双栏 1pt = 1/72inch   1 inch = 25.4mm
    xlabel('\fontname{宋体}力模长\fontname{Times New Roman}(N)','Fontname', 'Times New Roman');
    % xlabel('Torque $M_x$ (Nm)','interpreter','latex','FontSize',8); % Latex格式的label
    ylabel('\fontname{宋体}概率','Fontname', 'Times New Roman');
    set(gcf,'unit','points','PaperUnits','points','PaperPosition',[0,0,width,width/5*4],...
        'position',[500 300 width width/5*4],'color','w','PaperSize',[width, width/5*4]);
    
    set(gca, 'Fontsize', 8);   % 坐标轴句柄，设置坐标轴刻度数字样式
    L1 = legend([P1 P2 P3],'高斯分布','均值','阈值');
    set(L1,'Location','northeast','Orientation','vertical',...
        'Fontname', '宋体','FontSize',8,'TextColor','black');% legend 格式
    % set(gca, 'XTick',0:4:20);  % 设置X轴刻度
    
    print('S:\PbD\graph\Force_threhold_change','-djpeg','-r600');
    
legend 放置两排 需要鼠标拖动调整位置

    	L1 = legend([P1 P2 P3],'X\fontname{宋体}变扰动力','Y\fontname{宋体}变扰动力','Z\fontname{宋体}变扰动力');
    
    	set(L1,'Location','north','Orientation','horizontal',...
    	    'Fontname', 'Times New Roman','FontSize',8,'TextColor','black');% legend 格式
    	
    	ah=axes('position',get(gca,'position'),'visible','off');
    	L2=legend(ah,[P4 P5 P6],'X\fontname{宋体}恒扰动力','Y\fontname{宋体}恒扰动力',...
    		'Z\fontname{宋体}恒扰动力','orientation','horizontal','location','north');
    	
    	set(L2,    'Fontname', 'Times New Roman','FontSize',8,'TextColor','black');% legend 格式
    	
    	set(L1,'box','off');		% 根据需要可以去除legend边框
    	set(L2,'box','off');
去除图框白边	

    	set(gca,'looseInset',[0 0 0 0]);   	% 缩放坐标轴 去除图框白边 三维图不是很好用
设置label等对象位置两种方式

    	x = xlabel('\fontname{宋体}力模长\fontname{Times New Roman}(N)');
    	set(x,'Position',[0.1,0.1])
    	
    	xlabel('\fontname{宋体}力模长\fontname{Times New Roman}(N)','Position',[0.1,0.1]);


# 参考
1. [Matlab常用绘图命令](https://blog.csdn.net/weixin_38452468/article/details/90171772)
2. [MATLAB中的视角处理](https://blog.csdn.net/seamanj/article/details/35790359)     