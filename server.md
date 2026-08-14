# Server Script runs only on Hosted server & not in test mode
# KrunkScript Copyright (C) FRVR Limited
# 
# Add custom actions here

str[] admin = str["Sunnypatni112","ATL.06","stum2013020399","King_Bytel","Chen_Yu_1","ALG_VINIT"];
str[] root = str["slanik","Miekie","timeAndtime","Sumiji","archuselinux","superguy68","Guardian_1"];
str[] tmpAd = str[];
str[] tmpRo = str[];
str[] protAcc = str["Miekie"];

str[] btnIDs=str["mkAdBtnPlrs","mkAdBtnMt","mkAdBtnBn","mkAdBtnAd","mkAdBtnCon","mkAdBtnOth"];
str[] btnLbls=str["PLAYERS","MUTE","BAN","ADMINS","CONTROLS","OTHERS"];
str[] toolIDs=str["mkAdRytKick","mkAdRytBan","mkAdRytRevive","mkAdRytMute","mkAdRytGoTo","mkAdRytBring","mkAdRytPts100","mkAdRytPts1000","mkAdRytTempAd","mkAdRytTempRo"];
str[] toolLbls=str["KICK","BAN","REVIVE","MUTE","GO TO","BRING ME","+100pts","+1000pts","TEMP ADMIN","TEMP ROOT"];
str[] lmgs=str["MACHINE GUN","MINIGUN (n/a)"];
str[] smgs=str["SUBMACHINE GUN","AKIMBO UZI"];
str[] rifles=str["SNIPER RIFLE","ASSAULT RIFLE","FAMAS","SEMI AUTO"];
str[] laun=str["ROCKET","NOOB TUBE","WAR MACH (n/a)"];
str[] pistols=str["PISTOL","DESERT EAGLE","REVOLVER","TECHKY-9","AUTO PISTOL","AKIMBO PISTOL","ALIEN BLASTER"];
str[] shotguns=str["SHOTGUN","BLASTER","SAWED OFF"];
str[] special=str["SLIMER (n/a)","CHARGE RIFLE","CROSSBOW","COMBAT KNIFE","BOULDER","ZAPPER","COMPRESSOR"];
str[] tools=str["GRAPPLER","BUILD TOOL (n/a)"];

# - ADMIN PERMISSIONS - 
num[] adBtn=num[0]; # admin & tmp root/admin 
num[] adTool=num[0,1,4,5,3]; # admin & tmp admin

num[] rtBtn=num[0,1,2,3,4,5]; # root only
num[] rtTool=num[0,1,2,3,4,5,6,7,8,9]; # roott & tmp root

num[] lmgi=num[6,25];
num[] smgi=num[3,9];
num[] rifleI=num[0,1,14,7];
num[] launI=num[8,22,26];
num[] pistolI=num[2,10,4,21,16,27,11];
num[] shotI=num[5,18,15];
num[] specialI=num[23,28,13,12,29,24,30];
num[] toolI=num[20,19];
# - ADMIN PERMISSIONS - 

# REV NUKE TRIGGER LOCATION
num rnX = 889; num rnY = 147; num rnZ = 56;

obj[] svPlrLoc = obj[];

str r="#FF0000";
str wrng="#FFA600";
str info="#D1D1D1";
str HASH_CHARS = "0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz_.-";
obj[] adSess = obj[];

bool action inList(str[] arr, str v) {for (num i = 0; i < lengthOf arr; i++) {if (arr[i] == v) {return true; }}return false;}
obj action fnByID(str id) {return GAME.PLAYERS.findByID(id);}
obj[] action allPlr() {return GAME.PLAYERS.list();}
bool action inStrLs(str[] arr, str v) {for (num i = 0; i < lengthOf arr; i++) {if (arr[i] == v) {return true;}}return false;}
bool action idInAdSess(str id) {for (num i = 0; i < lengthOf adSess; i++) {if ((str)adSess[i].id == id) {return true;}}return false;}

bool action isRoot(str acc) {return inStrLs(root, acc);}
bool action isTmpRoot(str acc) {return inStrLs(tmpRo, acc);}
bool action hasRoot(str acc) {return isRoot(acc) || isTmpRoot(acc);}
bool action isAdmin(str acc) {return inStrLs(admin, acc) || inStrLs(tmpAd, acc);}
bool action isAuthorized(str acc) {return inStrLs(admin, acc) || inStrLs(tmpAd, acc) || hasRoot(acc);}
bool action isProtAcc(str acc) {return inStrLs(protAcc, acc);}

action netSd(str id, obj d, str pId) {GAME.NETWORK.send(id, d, pId);}
action netBc(str id, obj d) {GAME.NETWORK.broadcast(id, d);}

# C-style %s string formatter
action logR(str tag,str msg,str cl) {
 str[] chIgnr = str["NET", "REQ"];
 str t = "["+tag+"] ";
 for (num i = 0; i < lengthOf adSess; i++) {
  if (!inList(chIgnr, tag)) {GAME.CHAT.send(adSess[i].id,msg,cl);}
  if ((str)adSess[i].r == "ro") {netSd("lS",{m:t+msg},(str)adSess[i].id);}
 }
}

str[] plrLs = str[];
str[] plrLsID = str[];

action syncPlrLs(str id) {
 obj p=fnByID(id);
 if (!notEmpty p) {return;}
 str pId=(str)p.id;str pNm=(str)p.username;
 for (num i=0;i<lengthOf plrLsID;i++) {if (plrLsID[i]==pId) {return;}}
 addTo plrLs pNm;addTo plrLsID pId;
 for (num j=0;j<lengthOf adSess;j++) {netSd("plA",{n:pNm},(str)adSess[j].id);}
}

action syncPlrRm(str id) {
 for (num i=lengthOf plrLsID-1;i>=0;i--) {
  if (plrLsID[i]==id) {
   str pNm=plrLs[i];
   remove plrLsID[i];remove plrLs[i];
   for (num j=0;j<lengthOf adSess;j++) {netSd("plD",{n:pNm},(str)adSess[j].id);}
   break;
  }
 }
}

str[] banLs = str[
 "*xatrao*",
 "*xatroa*",
 "Bambuka",
 "zaku123",
 "memexur",
 "RIP_UZOK",
 "BinaryBeast",
 "Ciela-",
 "Spie4",
 "dik_scker",
 "-ThunderBird-",
 "xatroa010",
 "monkey_man_4",
 "bambukaaaa",
 "Gtx_1060",
 "strong_man_4",
 "dik_lover",
 "dildoer",
 "Domatici2000",
 "TheROOOOOOOK",
 "jUSTIN21908",
 "dik_in_ahh",
 "godofeclips",
 "eoyrdchf7G2r",
 "Saigonbac99",
 # "HOLA_SOY_LOAN",
 # "HOLA_SOY_LOAN2",
 "Yunus_E_1453",
 "noobino69",
 "MRAWEB23",
 "BigADawg",
 "KOOLKID923",
 # "NAVYSEAL_USA",
 "memexurer",
 "MiekeMapsSuck",
 "jUSTIN148920",
 "JuliBuli1899",
 "planetvenus22",
 "maskin12",
 "JuliBuli1899",
 "User_Guest_1",
 "Getzzy09",
 # "ELMER123LIN",
 "ergegegvewrge",
 "skibidimainoo",
 # "AshDarrings", # basher
 # "ZaainKrunk", # advertise
 "OuChhhh", # sus player
 # "Modrivc", # advertise
 "onFatha", # hacker/crasher
 # "KOOLKID91", # false accusation
 # "wq1l", # advertise
 "topthereal", # advertise
 "muhammadhi1", # lie and scam
 "Drukhari", # lie
 # "voozymantwo", # possible crasher
 "rankedrat", # muha's friend
 # "FloydBS", # muha's friend
 "Closey_567", # muha's friend
 # "val480", # muha's friend
 "MoTheCock", # muha's friend
 "r4p1t3c", # muha's friend
 "ohhellnahbro", # muha's friend
 "Floydvdv2000", # muha's friend
 "nortein8", # muha's friend
 "Jupiterrr7", # muha's friend
 "VigoPochmura", # muha's friend
 "Buff_Samsun", # muha's friend
 "OAUWE3520", # muha's alt
 "TGHERT7777", # aimbot
 "imnooblaptop",# crasher
 "IlIlllllIIIIlI"# crasher
];
str[] mtLs = str[
 #"AshDarrings",
 "Drukhari",
 "HOLA_SOY_LOAN",
 "HOLA_SOY_LOAN2",
 "KOOLKID91",
 "Modrivc",
 "topthereal",
 "wq1l",
 "ZaainKrunk",
 "Drukhari",
 "KOOLKID91",
 "FairsFR",
 "haui123",
 "luigiman",
 "DangerousNerd"
];
# -ADMIN SYSTEM-
# ban
action procBan(str id) {
 obj p = fnByID(id);
 if (!notEmpty p) {return;}
 for (num i = 0; i < lengthOf banLs; i++) {if (banLs[i] == (str)p.accountName) {GAME.ADMIN.ban(id);}}
}
# ban

# authentication
num action hashChar(str ch) {
 for (num i = 0; i < lengthOf HASH_CHARS; i++) {
  if (UTILS.truncateTxt(HASH_CHARS, i, true, i + 1) == ch) {return i;}}
  return 0;}

str action genSessID(str accountID, str accountName, str playerID) {
 str input = accountID + "|" + accountName + "|" + playerID + "|" + toStr(GAME.TIME.now());
 num h1 = 73129;num h2 = 19453;num h3 = 91771;
 for (num i = 0; i < lengthOf input; i++) {str ch = UTILS.truncateTxt(input, i, true, i + 1);
  num v = hashChar(ch);
  num n = 1000000007;
  h1 = (h1 * 257 + v + 17) % n;
  h2 = (h2 * 131 + v + h1) % n;
  h3 = (h3 * 193 + v + h2) % n;
 }
 num a = h1 % 64;num b = h2 % 64;num c = h3 % 64;num d = (h1 + h2) % 64;num e = (h2 + h3) % 64;num f = (h1 + h3) % 64;
 return (
  UTILS.truncateTxt(HASH_CHARS, a, true, a + 1) +UTILS.truncateTxt(HASH_CHARS, b, true, b + 1) + UTILS.truncateTxt(HASH_CHARS, c, true, c + 1) +
  UTILS.truncateTxt(HASH_CHARS, d, true, d + 1) +UTILS.truncateTxt(HASH_CHARS, e, true, e + 1) + UTILS.truncateTxt(HASH_CHARS, f, true, f + 1)
 );
}

action getByIdx(str[] dst,str[] src,num[] idx) {for (num i=0;i<lengthOf idx;i++) {addTo dst src[idx[i]];}}

action admAuth(str id) {
 obj p=fnByID(id);
 if (!notEmpty p || !isAuthorized((str)p.accountName)) {return;}

 if (!idInAdSess((str)p.id)) {
  str acNm=(str)p.accountName;
  str pId=(str)p.id;
  str sessID=genSessID((str)p.accountID,acNm,pId);
  str rl="ad";
  str[] b=str[];str[] c=str[];str[] t=str[];str[] u=str[];
  str[] l=str[];str[] s=str[];str[] r=str[];str[] x=str[];str[] q=str[];str[] h=str[];str[] z=str[];str[] w=str[];

 if (hasRoot(acNm)) {
  if (isRoot(acNm)) {rl="ro";getByIdx(b,btnIDs,rtBtn);getByIdx(c,btnLbls,rtBtn); }
  else {rl="tr";getByIdx(b,btnIDs,adBtn);getByIdx(c,btnLbls,adBtn);
  }
  getByIdx(t,toolIDs,rtTool);getByIdx(u,toolLbls,rtTool);
  l=lmgs;s=smgs;r=rifles;x=laun;q=pistols;h=shotguns;z=special;w=tools;
 }
 else {
  if (inStrLs(tmpAd,acNm)) {rl="ta";}
  getByIdx(b,btnIDs,adBtn);getByIdx(c,btnLbls,adBtn);
  getByIdx(t,toolIDs,adTool);getByIdx(u,toolLbls,adTool);
 }

  addTo adSess {id:pId,accN:acNm,sId:sessID,r:rl};
  netSd("sID",{k:sessID, r: rl},pId);
  netSd("aUI",{b:b,c:c,t:t,u:u,l:l,s:s,r:r,x:x,p:q,h:h,z:z,w:w},pId);
 }
}

action rmAdSess(str id) {
 for (num i = 0; i < lengthOf adSess; i++) {
  if ((str)adSess[i].id == id) {remove adSess[i]; break;}}
}
bool action verSessId(str sdr, str sID) {
 for (num i = 0; i < lengthOf adSess; i ++) {
  if ((str)adSess[i].id == sdr && (str)adSess[i].sId == sID) {return true;}
 }
 return false;
}

action invalidAdReq(str sdr, str acc) {rmAdSess(sdr);logR("nVER","INVALID REQUEST sent by "+acc,r);}

bool action verAd(str sdr, str sId) {
 obj p = fnByID(sdr);if (!notEmpty p) {return false;}
 str acc = (str)p.accountName;
 if (!verSessId(sdr,sId) || !isAuthorized(acc)) {invalidAdReq(sdr,acc);return false;}
 return true;
}
# authentication

# authorization
bool action procActPerm(str a, str id, str t) {
 bool allo = true;
 if (isRoot(t)) {allo = false;}
 else if (!isRoot(a) && isProtAcc(t)) {allo = false;}
 else if (isTmpRoot(a) && isTmpRoot(t)) {allo = false;}
 else if (isAdmin(a) && isTmpRoot(t)) {allo = false;}
 else if (isAdmin(a) && isAdmin(t)) {allo = false;}
 if (!allo) {logR("INFO",a+" DENIED "+id+" on "+t,info);}
 return allo;
}
# authorization

# network security
obj[] rlRec = obj[];
bool action rlLogged(str id) {
 for (num i=0;i<lengthOf rlRec;i++) {
  if ((str)rlRec[i].id==id) {
   if ((bool)rlRec[i].log) {return true;}
   rlRec[i].log=true;
   return false;
  }
 }
 return false;
}
bool action allowReq(str id) {
 num now=GAME.TIME.now();

 for (num i=0;i<lengthOf rlRec;i++) {
  if ((str)rlRec[i].id==id) {

   if (now-(num)rlRec[i].tm>=1000) {
    rlRec[i].ct=1;
    rlRec[i].tm=now;
    rlRec[i].exp=now+5000;
    rlRec[i].log=false;
    return true;
   }

   (num)rlRec[i].ct+=1;

   if ((num)rlRec[i].ct>5) {
    return false;
   }

   return true;
  }
 }

 addTo rlRec {id:id,ct:1,tm:now,exp:now+5000,log:false};
 return true;
}
action rmRL(str id) {
 for (num i = lengthOf rlRec - 1; i >= 0; i--) {
  if ((str)rlRec[i].id == id) {remove rlRec[i];}
 }
}
action rlTime(num delta) {
 num now = GAME.TIME.now();
 for (num i = lengthOf rlRec - 1; i >= 0; i--) {
  if (now >= (num)rlRec[i].exp) {remove rlRec[i];}
 }
}

num action validAdPkt(str id, obj data, obj p) {
 if ((str)data.sI == "") {return 0;}

 str[] btn = str["bPl","bBn","bMt"];
 if (inList(btn,id)) {return 1;}

 if ((str)data.tU == "" || (str)data.w == "") {return 0;}
 str[] act=str["kc","bn","mt","rv","gt","bm","1h","1t","ta","tr","aW"];
 if (inList(act,id)) {return 2;}
 logR("WRNG",(str)p.accountName+" sent invalid network request: "+id,wrng);
 return 0;
}
# network security

# data request
str[] dtRq = str[];

bool action dtReqUsed(str pId,str type) {
 str k=pId+"|"+type;
 for (num i=0;i<lengthOf dtRq;i++) {if (dtRq[i]==k) {return true;}}
 addTo dtRq k;
 return false;
}

action procDtReq(str id,obj data,str pId) {
 str sId=(str)data.sI;
 if (!verAd(pId,sId)) {return;}
 str type="";str res="";

 if (id=="bPl") {type="pl";res="plL";}
 if (id=="bMt") {type="mt";res="mtL";}
 if (id=="bBn") {type="bn";res="bnL";}
 if (id=="bAd") {type="ad";res="adL";}
 if (id=="bVp") {type="vp";res="vpL";}

 if (type=="") {return;}
 if (dtReqUsed(pId,type)) {netSd(res,{d:""},pId);return;}

 obj s=fnByID(pId);
 if (!notEmpty s) {return;}
 if (type=="pl") {netSd("plL", {d: plrLs}, pId);}
 if (type=="mt") {netSd("mtL", {d: mtLs}, pId);	}
 if (type=="bn") {netSd("bnL", {d: banLs}, pId);}
 logR("REQ",(str)s.accountName+" req "+id+" data",info);
}

action rmDtRq(str pId) {
 for (num i=lengthOf dtRq-1;i>=0;i--) {
  if (UTILS.truncateTxt(dtRq[i],0,true,lengthOf pId)==pId) {
   remove dtRq[i];
  }
 }
}
# data request

# action security
action grantTmpRole(str role, str sAcc, str tAcc, str trg) {
 if (isAuthorized(tAcc)) {logR("ACT",tAcc+" is already an Admin",info);return;}
 if (!inStrLs(role == "ta" ? tmpAd : tmpRo,tAcc)) {addTo (role == "ta" ? tmpAd : tmpRo) tAcc;}
 admAuth(trg);
 logR("ACT",sAcc+" granted "+(role == "ta" ? "TMP ADMIN" : "TMP ROOT")+" to "+tAcc,info);
}
action setPlrTeam(obj t) {
 addTo svPlrLoc {id: (str)t.id,x: (num)t.position.x,y: (num)t.position.y,z: (num)t.position.z,at: GAME.TIME.now() + 500};
 t.position.x = rnX;t.position.y = rnY;t.position.z = rnZ;

}

num action getWepID(str n) {
 for (num i=0;i<7;i++) {
  if (i<lengthOf lmgs && n==lmgs[i]) {return lmgi[i];}
  if (i<lengthOf smgs && n==smgs[i]) {return smgi[i];}
  if (i<lengthOf rifles && n==rifles[i]) {return rifleI[i];}
  if (i<lengthOf laun && n==laun[i]) {return launI[i];}
  if (i<lengthOf pistols && n==pistols[i]) {return pistolI[i];}
  if (i<lengthOf shotguns && n==shotguns[i]) {return shotI[i];}
  if (i<lengthOf special && n==special[i]) {return specialI[i];}
  if (i<lengthOf tools && n==tools[i]) {return toolI[i];}
 }
 return -1;
}

action giveWep(obj p, str a, str n, str t) {
 num id=getWepID(n);
 if (id>=0) {p.giveWeapon(id);}
 logR("WEP",a+" req "+n+" for "+t,info);
}

action procAdAct(str id, obj data, str pID) {
 str sId = (str)data.sI; # admin's sess id
 str sdr = pID; # sender's id
 if (!verAd(sdr, sId)) {return;}

 str tUsr = (str)data.tU; # target's usrname
 str trg = ""; # target's uid
 obj[] plr = allPlr();
 for (num i = 0; i < lengthOf plr; i ++) {
  if ((str)plr[i].username == tUsr) {
   trg = (str)plr[i].id; 
   if (trg=="") {return;}break;}
 }
 
 obj s = fnByID(sdr);obj t = fnByID(trg);
 if (!notEmpty s || !notEmpty t) {return;}
 str sAcc = (str)s.accountName;str tAcc = (str)t.accountName;
 if (tAcc == "") {tAcc = (str)t.username;}

 if (id == "aW") {giveWep(t, sAcc, (str)data.w, tAcc); return;}
 if (id == "rv") {setPlrTeam(t);logR("ACT",sAcc+" req REVIVE for "+tAcc,info);return;}

 if (id == "gt"){logR("ACT",sAcc+" req GO TO for "+tAcc,info);return;}
 if (id == "bm"){logR("ACT",sAcc+" req BRING ME for "+tAcc,info);return;}

 if (id == "1h") {(num)t.score += 100;logR("ACT",sAcc+" req +100pts for "+tAcc,info);return;}
 if (id == "1t") {(num)t.score += 1000;logR("ACT",sAcc+" req +1000pts for "+tAcc,info);return;}
 if (id == "ta") {grantTmpRole("ta",sAcc,tAcc,trg);return;}
 if (id == "tr") {grantTmpRole("tr",sAcc,tAcc,trg);return;}

 if (!procActPerm(sAcc,id,tAcc)) {return;}
 str lc = ""; str act = "";
 if (id == "kc") {
  GAME.ADMIN.kick(trg);lc="#a83841";act="kicked";
 }
 if (id == "bn") {
  addTo banLs tAcc; GAME.ADMIN.ban(tAcc);lc="#991d24";act="banned";
 }
 if (id == "mt") {
	
 }
 logR("INFO",sAcc+" GRANTED "+id+" on "+tAcc,info);
}
# action security

# -ADMIN SYSTEM-

# Runs when the game starts
public action start() {
}

# Runs every game tick
public action update(num delta) {
 rlTime(delta);
 if (lengthOf svPlrLoc != 0) {
  num now = GAME.TIME.now();
  for (num i = lengthOf svPlrLoc - 1; i >= 0; i--) {
   if (now >= (num)svPlrLoc[i].at) {obj p = GAME.PLAYERS.findByID((str)svPlrLoc[i].id);
	if (notEmpty p) {p.position.x = (num)svPlrLoc[i].x;p.position.y = (num)svPlrLoc[i].y;p.position.z = (num)svPlrLoc[i].z;}
	remove svPlrLoc[i];}
  } 
 }
}

# Player spawns in
public action onPlayerSpawn(str id) {
 obj p = fnByID(id);
 if (!notEmpty p) {return;}
 # if (!isAdmin((str)p.accountName)) {GAME.ADMIN.ban(id);}
 syncPlrLs(id);
 admAuth(id);
 procBan(id);
}

# Player died
public action onPlayerDeath(str id, str killerID) {

}

# Player got damaged
public action onPlayerDamage(str id, str doerID, num amount) {

}

# Player update
public action onPlayerUpdate(str id, num delta, obj inputs) {

}

# Called from Custom Trigger Action
public action onCustomTrigger(str playerID, str customParam, num value) {

}

# Should trigger a trigger? Return true or false
public bool action shouldTrigger(str playerID, str triggerID, str customParam) {
 return true;
}

# Server receives network message
public action onNetworkMessage(str id, obj data, str pID) {
 obj p = fnByID(pID);
 if (!notEmpty p) {return;}

 if (!allowReq(pID)) {
  if (!rlLogged(pID)) {
   logR("WRNG",(str)p.accountName+" exceeded network request limit",wrng);
  }
  return;
 }
 num vld = validAdPkt(id, data, p);
 if (vld == 0) {return;}
 logR("NET",(str)p.accountName+" sent REQ to server","");
 if (vld == 1 && (str)data.r == "rq") {procDtReq(id, data, pID);}
 if (vld == 2) {procAdAct(id, data, pID);}
 if (vld >= 5) {
 # alert super admins but do not remove session id for live server investigation
 }
}

# Server receives chat message
public action onChatMessage(str msg, str playerID) {

}

# When a player leaves the server
public action onPlayerLeave(str playerID) {
 rmRL(playerID);
 rmAdSess(playerID);
 syncPlrRm(playerID);
 rmDtRq(playerID);
}

# Runs when the round ends
public action onGameEnd() {

}

# When a player finished a video
public action onAdFinished(str playerID, bool success) {

}

# Runs when the server closes
public action onServerClosed() {

}

# When a deposit box is changed
public action onDepositBoxChange(str objectID, str playerID, num amount, num finalAmount) {

}