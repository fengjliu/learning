Compile electron main.ts and copy output to dist folder,

ng build --progress=false --configuration production --base-href=./
    (该命令 ng build --progress=false --configuration production --base-href=./ 是用于使用 Angular CLI 构建生产环境的应用程序。

    以下是各个参数的说明：

    --progress=false：禁用构建进度显示，以简化输出信息。
    --configuration production：使用 production 配置来进行构建，这通常会启用优化和压缩等生产环境特定的设置。
    --base-href=./：指定应用程序的基本 URL 路径，这对于部署到相对路径的位置很有用。
    执行该命令后，Angular CLI 将根据项目配置进行构建，并在生产模式下生成优化的输出文件。生成的文件将位于输出目录中，该目录通常是项目根目录下的 dist 文件夹。

    请确保在执行该命令之前，你已经安装了 Angular CLI，并且在当前工作目录下存在正确配置的 Angular 项目。)
Copy backend native folder into dist foler

npm run build:libs:  compile front-end and copy output into dist folder

in Dist foler:  electron-packager dist Awu --overwrite --icon=src/keysight-logo.ico --platform=win32 --arch=x64 --asar.unpack=\"**/native/bin/**/*\"'

Zip:  
  # Zip it all up
  print('Zip up the output')
  cmd = ''
  if sys.platform == 'win32':
    cmd = 'powershell.exe -NoProfile -Command "Compress-Archive -Path ./Awu-*-x64/* -CompressionLevel Optimal -DestinationPath ./{}"'.format(output_file)
  else:
    cmd = 'tar -czvf {}.tgz ./Awu-*-x64'.format(output_file)
   
   

http://127.0.0.1:2999/processlist/api/v1/outputmodels
