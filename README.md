# bitcoin-windows-server

Windows Service wrapper for bitcoind (Bitcoin core daemon), to run it automatically on a server.


## Usage

For help, use .\BitcoinService.exe /help

1. Install Bitcoin Core; this should install into "C:\Program Files\Bitcoin\daemon"
2. Create a data directory, e.g. F:\Bitcoin
3. Copy your bitcoin.conf file into F:\Bitcoin, make sure you have set rpcuser and rpcpassword
4. Create a directory for the service, e.g. F:\BitcoinService
5. Copy BitcoinService.exe and BitcoinService.exe.config into F:\BitcoinService
6. Install the service via .\BitcoinService.exe /install "-testnet -datadir=F:\Bitcoin"
7. Start the service via Get-Service bitcoind | Start-Service
8. Check you can connect to the service via RPC

   & "C:\Program Files\Bitcoin\daemon\bitcoin-cli.exe" -testnet -rpcuser=bitcoinrpc -rpcpassword=[your_rpc_password] getinfo 

   
## Troubleshooting

### Testing

If you need to test, try running bitcoind.exe directly, to test your arguments, e.g.

  & "C:\Program Files\Bitcoin\daemon\bitcoind.exe" -datadir=F:\Bitcoin -testnet

Open a second window to test bitcoin-cli.exe commands.

If this isn't working, then it is a problem with your Bitcoin Core installation.

### Security issues

If the install works (writes to the log), but Start-Service fails immediately 
(without any logging), then the service account may not have permission to write 
to the log (the first thing it does).

Check the security on the F:\BitcoinService\Logs folder; you may need to add 
Local Service with Modify permissions.
  
If you still see errors, e.g. EXITED: 1 (meaning the bitcoind.exe process started 
then exited immediately), then check the permissions on the F:\Bitcoin data directory; 
you may need to add Local Service with Modify permissions there as well.   


## History

Originally inspired by a message post by mb300sd in 2011 at https://bitcointalk.org/index.php?topic=43446.0

Code (which no longer worked) was at http://www.mediafire.com/?cyy1lqp6xo9042h

The message also stated "Donations welcome if you find it useful, 5BTC required if you use it commercially. 1D7FJWRzeKa4SLmTznd3JpeNU13L1ErEco"

One comment was to simply use instsrv.exe / srvany.exe, although there was a discussion this doesn't shut down bitcoind cleanly.

Another users, rjk, added a message with an improvement to work with other coins, but the link is broken.

Note that the original wrapper tried to send the command "bitcoind.exe stop" to close down, however that no longer works due to security issues, so just srvany.exe is probably now sufficient.
