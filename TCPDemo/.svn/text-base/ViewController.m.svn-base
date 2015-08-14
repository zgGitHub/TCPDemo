//
//  ViewController.m
//  TCPDemo
//
//  Created by fq on 15/8/13.
//  Copyright (c) 2015年 fangqian. All rights reserved.
//

#import "ViewController.h"
#import "AsyncSocket.h"
@interface ViewController ()<AsyncSocketDelegate,UITextFieldDelegate>
{
    //负责显示接收到的数据
    UITextView * _textView;
    //负责输入数据
    UITextField * _textField;
    //建立发送端
    AsyncSocket * sendSocket;
    //建立服务端
    AsyncSocket * severSocket;
    
    
}
//建立一个数组保存连接
@property(nonatomic,strong)NSMutableArray * socketArray;
@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view, typically from a nib.
    /*
       建立一个群聊，学生向教师端发送消息，教师端显示所有消息
     */
    
    self.socketArray=[NSMutableArray arrayWithCapacity:0];

    [self CreatSocket];
    
    //创建界面
    [self CreatView];
    //定时器,每隔10s钟清除数据
    [NSTimer scheduledTimerWithTimeInterval:10 target:self selector:@selector(clearTextView) userInfo:nil repeats:YES];
}

- (void)clearTextView{
    _textView.text=nil;
}

- (void)CreatView{
    
    _textView=[[UITextView alloc] initWithFrame:CGRectMake(0, 0, self.view.frame.size.width, 240)];
    _textView.backgroundColor=[UIColor blackColor];
    _textView.textColor=[UIColor whiteColor];
    [self.view addSubview:_textView];
    
    _textField=[[UITextField alloc] initWithFrame:CGRectMake(0, 240, self.view.frame.size.width, 44)];
    _textField.delegate=self;
    _textField.backgroundColor=[UIColor redColor];
    [self.view addSubview:_textField];
    
}

- (void)CreatSocket
{
    sendSocket=[[AsyncSocket alloc] initWithDelegate:self];
    severSocket=[[AsyncSocket alloc] initWithDelegate:self];
    //服务端绑定端口，监听该端口接收的数据
    /*
       端口最大为65535，其中建议设置为5000以上，另外还有一些特殊的端口，例如8080为视频端口，建议不要占用
     */
    [severSocket acceptOnPort:5678 error:nil];
    
    
}
- (void)onSocket:(AsyncSocket *)sock didAcceptNewSocket:(AsyncSocket *)newSocket
{
  //接收的一个新连接，这个连接需要保存一下，然后持续保持连接
    [self.socketArray addObject:newSocket];
    
    //其中-1标示持续观察，如果设置为300，那么300秒以后就不在观察
    [newSocket readDataWithTimeout:-1 tag:100];
    
    
}
- (void)onSocket:(AsyncSocket *)sock didReadData:(NSData *)data withTag:(long)tag
{
   //接收到的数据
    NSString * message=[[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding];
    if (_textView) {
        //在原来的旧数据上面，追加新的数据
        _textView.text=[NSString stringWithFormat:@"%@%@",_textView.text,message];
    }
    
    [sock readDataWithTimeout:-1 tag:100];
    
}

- (void)onSocket:(AsyncSocket *)sock didWriteDataWithTag:(long)tag
{
 //发送成功
}

//发送数据
-(BOOL)textFieldShouldReturn:(UITextField *)textField{
    if (textField.text.length>0) {
        // 发送数据
        if (![sendSocket isConnected]) {
            //确定是否连接，如果没有连接，则开始连接
            [sendSocket connectToHost:@"192.168.2.7" onPort:5678 error:nil];
        }
        //当连接完成以后，发送数据
        //拼接数据是谁说，我希望获得当前设备的名称
       // [[UIDevice currentDevice]systemName];该方法只有在真机上才有效，在模拟器上无效
        NSString * message=[NSString stringWithFormat:@"%@说：%@\n",@"房骞",textField.text];
        [sendSocket writeData:[message dataUsingEncoding:NSUTF8StringEncoding] withTimeout:-1 tag:100];
   
    }
    return YES;
}














- (void)didReceiveMemoryWarning {
    [super didReceiveMemoryWarning];
    // Dispose of any resources that can be recreated.
}

@end
