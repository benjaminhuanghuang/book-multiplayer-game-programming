




## 3.4 UDP Socket





## 3.5 TCP Socket





## 3.6 Blocking and Non-blocking I/O
There are three common ways to work around blocking issue: 
multithreading, non-blocking I/O, and the select function.



### Non-blocking I/O
By default sockets operate in blocking mode



int UDPSocket::SetNonBlockingMode(bool inShouldBeNonBlocking)
{
#if _WIN32
u_long arg = inShouldBeNonBlocking ? 1 : 0;
int result = ioctlsocket(mSocket, FIONBIO, &arg);
#else
int flags = fcntl(mSocket, F_GETFL, 0);
flags = inShouldBeNonBlocking ?
(flags | O_NONBLOCK):(flags & ~O_NONBLOCK);
fcntl(mSocket, F_SETFL, flags);
#endif
if(result == SOCKET_ERROR)
{
SocketUtil::ReportError(L"UDPSocket::SetNonBlockingMode");
return SocketUtil::GetLastError();
}
else
{
return NO_ERROR;
}
}


Game Loop Using a Non-Blocking Socket
void DoGameLoop()
{
  UDPSocketPtr mySock = SocketUtil::CreateUDPSocket(INET);
  mySock->SetNonBlockingMode(true);
  while(gIsGameRunning)
  {
  char data[1500];
  SocketAddress socketAddress;
  int bytesReceived = mySock->ReceiveFrom(data, sizeof(data),
  socketAddress);
  if(bytesReceived> 0)
  {
  ProcessReceivedData(data, bytesReceived, socketAddress);
  }
  DoGameFrame();
  }
}


### Select
Polling non-blocking sockets each frame is a simple and straightforward way to check for
incoming data without blocking a thread. 
However, when the number of sockets to poll is large,
this can become inefficient.