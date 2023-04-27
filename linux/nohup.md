
nohup Command [ Arg … ] 
不挂断地运行命令，即退出了终端仍然会在后台运行，常配合&使用

nohup jupyter notebook --allow-root > jupyter.log 2>&1 & : 脱离终端