# Client Script runs only on the client
# KrunkScript Copyright (C) FRVR Limited
# 
# Add custom actions here

# -ADMIN SYSTEM-
str k = "";
str r = "";
str[] bnLs = str[];
str[] mtLs = str[];
str[] vipLs = str[];
str[] plrLs = str[];
str[] adminLs = str[];

bool adminPanelOpen = false;
str bg = "background:rgba(0,0,0,0.5);";
str bg1 = "background:rgba(35,35,35,0.5);";
str bg2 = "background:rgba(153,29,36,1);";
str fnS = "font-size:28px;";
str fnS1 = "font-size:15px;";
str fnS2 = "font-size:12px;";
str fnS3 = "font-size:10px;";

str fnC = "color:#ffffff;";
str fnC1 = "color:#000000;";
str fnPlr = "font-size:15px;font-weight:400;";

str brd = "border:1px solid rgba(255,255,255,0.75);";
str brdRad = "border-radius:10px;";
str brdRad1 = "border-radius:20px;";
str txC = "#ffffff;";
str ff = "font-weight:800;";
str cr = "border-radius:10px;";
str bx = "box-sizing:border-box;";
str ov = "overflow:hidden;";
str z = "z-index:9999;";
str ps = "position:absolute;";
str txAlgCen = "text-align:center;";
str st = ps + bx + ov;

str adSelRowBg = "rgba(55,55,55,1)";
str adSelRowBrd = "2px solid rgba(255,255,255,1)";
str adNorRowBg = "rgba(0,0,0,0.5)";
str adNorRowBrd = "1px solid rgba(255,255,255,0.75)";
str adSelPlr = "";

str adSelBtn = "";
str adSelLst = "";

str[] adminButtonIDs = str[];
str[] adminButtonLabels = str[];
str[] toolIDs = str[];
str[] toolLabels = str[];
str[] lmgs = str[];
str[] smgs = str[];
str[] rifles = str[];
str[] launchers = str[];
str[] pistols = str[];
str[] shotguns = str[];
str[] special = str[];
str[] tools = str[];

str btnBg = "rgba(55,55,55,0.5)";
str selBg = "rgba(75,75,75,1)";
str norBrd = "1px solid rgba(255,255,255,0.5)";
str selBrd = "3px solid rgba(255,255,255,1)";

str lnHght = "line-height:42px;";
str[] toolBg = str["rgba(168,56,65,1)","rgba(153,29,36,1)","rgba(54,54,54,1)","rgba(54,54,54,1)","rgba(54,54,54,1)","rgba(54,54,54,1)","rgba(54,54,54,1)","rgba(54,54,54,1)","rgba(20,144,170,1)","rgba(20,144,170,1)"];
str bgBtn = "rgba(17,19,42,1)";
# global/shared
action netSd(str id, obj d) {GAME.NETWORK.send(id, d);}
action updDIV(str id, str prop, str v){GAME.UI.updateDIV(id, prop, v);}
action updDIVTxt(str id, str txt){GAME.UI.updateDIVText(id, txt);}
action crtDIV(str id, bool vis, str css, str parentId){GAME.UI.addDIV(id, vis, css, parentId);}
action assgSess(obj data) {k = (str)data.k; r = (str)data.r;}
action logS(obj data) {GAME.log((str)data.m);}

# global/shared



# left panel
action crtAdBtn(str id, str label, num top) {
 crtDIV(id, true, "position:absolute;left:20px;top:" + toStr(top) + "px;width:260px;height:50px;background:" + btnBg + ";border:" + norBrd + ";border-radius:10px;" +
 "box-sizing:border-box;"+fnC+fnS1+txAlgCen+"line-height:52px;cursor:pointer;pointer-events:auto;", "mkAdLft");
 updDIVTxt(id, label);
 #if (id == adSelBtn) {updDIV(id, "border", selBrd);updDIV(id,"background",selBg);}
}

action selAdBtn(str id) {
 if (id == adSelBtn) {return;}
 if (adSelBtn != "") {updDIV(adSelBtn,"border",norBrd);updDIV(adSelBtn,"background",btnBg);}
 updDIV(id,"border",selBrd);updDIV(id,"background",selBg);
 adSelBtn = id;
}
#left panel

# right panel

str adSelRyt = "";
str adSelRytBrd = "3px solid rgba(255,255,255,1)";
str[] wepBtnIDs = str[];
str[] wepNm = str[];
str adSelWep = "";

action clrAdRytSel() {
 if (adSelRyt != "") {
  updDIV(adSelRyt,"border",norBrd);

  if (adSelRyt == "mkAdRytBan") {
   updDIVTxt(adSelRyt,"BAN");
  }
 }

 adSelRyt = "";
}

action selAdRyt(str id,str act,str lbl) {
 if (adSelRyt != "") {
  updDIV(adSelRyt,"border",norBrd);

  if (adSelRyt == "mkAdRytBan") {updDIVTxt(adSelRyt,"BAN");}
  if (adSelRyt == "mkAdRytMuteAction" || adSelRyt == "mkAdRytBanAction") {updDIVTxt(adSelRyt,"REMOVE");}
 }

 if (adSelWep != "") {
  updDIV(adSelWep,"border",norBrd);
  adSelWep = "";
 }

 if (id == "mkAdRytBan" || id == "mkAdRytMuteAction" || id == "mkAdRytBanAction") {
  if (adSelRyt == id) {
   netSd(act,{sI:k,tU:adSelPlr});
   updDIVTxt(id,lbl);
   adSelRyt = "";
   return;
  }

  updDIV(id,"border",adSelRytBrd);
  updDIVTxt(id,"CONFIRM?");
  adSelRyt = id;
  return;
 }

 updDIV(id,"border",adSelRytBrd);
 adSelRyt = id;
 netSd(act,{sI:k,tU:adSelPlr});
}

action procAdRytAct(str id) {
 if (id == "mkAdRytKick") {selAdRyt(id,"kc","KICK");return;}
 if (id == "mkAdRytBan") {selAdRyt(id,"bn","BAN");return;}
 if (id == "mkAdRytMute") {selAdRyt(id,"mt","MUTE");return;}
 if (id == "mkAdRytRevive") {selAdRyt(id,"rv","REVIVE");return;}
 if (id == "mkAdRytGoTo") {selAdRyt(id,"gt","GO TO");return;}
 if (id == "mkAdRytBring") {selAdRyt(id,"bm","BRING ME");return;}
 if (id == "mkAdRytPts100") {selAdRyt(id,"1h","+100pts");return;}
 if (id == "mkAdRytPts1000") {selAdRyt(id,"1t","+1000pts");return;}
 if (id == "mkAdRytTempAd") {selAdRyt(id,"ta","TEMP ADMIN");return;}
 if (id == "mkAdRytTempRo") {selAdRyt(id,"tr","TEMP ROOT");return;}

 if (id == "mkAdRytMuteAction") {selAdRyt(id,"rM","REMOVE");return;}
 if (id == "mkAdRytBanAction") {selAdRyt(id,"rB","REMOVE");return;}
}

action selAdWep(str id, str name) {
 if (adSelWep != "") {updDIV(adSelWep,"border",norBrd);}
 if (adSelRyt != "") {updDIV(adSelRyt,"border",norBrd);adSelRyt = "";}
 updDIV(id,"border","3px solid rgba(255,255,255,1)");
 adSelWep = id;
 netSd("aW", {sI:k,tU:adSelPlr,w:name});
}

action procAdWep(str id) {
for (num i = 0; i < lengthOf wepBtnIDs; i++) {
 if (id == wepBtnIDs[i]) {
	 
  selAdWep(id, wepNm[i]);
  return;
 }
}
}

action clrAdRyt() {
 GAME.UI.removeDIV("mkAdRytBox");

 crtDIV("mkAdRytBox",true,
  "position:absolute;left:0;top:0;width:100%;height:100%;box-sizing:border-box;",
  "mkAdRyt"
 );

 crtDIV("mkAdRytEmpty",true,
  "position:absolute;left:20px;top:50%;width:280px;height:24px;box-sizing:border-box;"+fnC+fnS1+txAlgCen+"line-height:24px;pointer-events:none;",
  "mkAdRytBox"
 );

 updDIVTxt("mkAdRytEmpty","CURRENTLY EMPTY");
}

action crtAdRyt() {
 crtDIV("mkAdRytBox",true,
 "position:absolute;left:0;top:0;width:100%;height:100%;box-sizing:border-box;",
 "mkAdRyt"
 );
}

action crtAdRytBtn(str id, str label, num left, num top, str bg, str fn) {
 crtDIV(id,true,
 "position:absolute;left:"+toStr(left)+"px;top:"+toStr(top)+"px;width:130px;height:40px;box-sizing:border-box;background:"+bg+";"+brd+brdRad+fnC+fn+txAlgCen+lnHght+"cursor:pointer;pointer-events:auto;",
 "mkAdRytTools"
 );

 updDIVTxt(id,label);
}

action crtAdRytWep(str title, str[] weapons, num left, num top) {
 crtDIV("mkAdRyt"+title+"Title",true,
  "position:absolute;left:"+toStr(left)+"px;top:"+toStr(top)+"px;width:130px;height:20px;box-sizing:border-box;"+fnC+fnS3+ff+"line-height:20px;pointer-events:none;","mkAdRytTools");

 updDIVTxt("mkAdRyt"+title+"Title",title);

 for (num i = 0; i < lengthOf weapons; i++) {
  str id = "mkAdRyt"+title+"_"+toStr(i);

  addTo wepBtnIDs id;
  addTo wepNm weapons[i];
  crtAdRytBtn(id,weapons[i],left,top + 25 + (i * 50),bgBtn,fnS3);
 }
}

action adActNTools() {
 wepBtnIDs = str[];
 wepNm = str[];

 crtDIV("mkAdRytTools",true,
 "position:absolute;left:0;top:170px;width:100%;height:260px;box-sizing:border-box;",
 "mkAdRytBox"
 );

 crtDIV("mkAdRytToolTitle",true,
 "position:absolute;left:20px;top:0;width:280px;height:20px;box-sizing:border-box;"+fnC+fnS2+ff+"line-height:20px;pointer-events:none;",
 "mkAdRytTools"
 );

 updDIVTxt("mkAdRytToolTitle","ADMIN TOOLS");

 for (num i = 0; i < lengthOf toolIDs; i++) {
  num row = i - (i % 2);
  crtAdRytBtn(toolIDs[i],toolLabels[i],20 + ((i % 2) * 150),30 + (row * 25),toolBg[i], fnS2);
 }
 if (r != "ro" && r != "tr") {return;}
 crtDIV("mkAdRytWeaponTitle",true,
 "position:absolute;left:20px;top:300px;width:280px;height:20px;box-sizing:border-box;"+fnC+fnS2+ff+"line-height:20px;pointer-events:none;",
 "mkAdRytTools"
 );

 updDIVTxt("mkAdRytWeaponTitle","ADMIN WEAPONS");

crtAdRytWep("LMGs",lmgs,20,330);
crtAdRytWep("SMGs",smgs,170,330);
crtAdRytWep("RIFLES",rifles,20,470);
crtAdRytWep("LAUNCHERS",launchers,170,470);
crtAdRytWep("PISTOLS",pistols,20,710);
crtAdRytWep("SHOTGUNS",shotguns,170,660);
crtAdRytWep("TOOLS",tools,20,1100);
crtAdRytWep("SPECIAL",special,170,850);
}


action adMtAct() {
 crtDIV("mkAdRytMuteAction",true,
  "position:absolute;left:95px;top:170px;width:130px;height:40px;box-sizing:border-box;"+bg2+brd+brdRad+fnC+fnS2+txAlgCen+lnHght+"cursor:pointer;pointer-events:auto;",
  "mkAdRytBox"
 );

 updDIVTxt("mkAdRytMuteAction","REMOVE");
}

action adBnAct() {
 crtDIV("mkAdRytBanAction",true,
  "position:absolute;left:95px;top:170px;width:130px;height:40px;box-sizing:border-box;"+bg2+brd+brdRad+fnC+fnS2+txAlgCen+lnHght+"cursor:pointer;pointer-events:auto;",
  "mkAdRytBox"
 );

 updDIVTxt("mkAdRytBanAction","REMOVE");
}

action updAdRytPlr(str pNm, str prefix) {
 GAME.UI.removeDIV("mkAdRytBox");

 crtDIV("mkAdRytBox",true,
 "position:absolute;left:0;top:20px;width:100%;height:calc(100% - 40px);box-sizing:border-box;overflow-y:auto;overflow-x:hidden;",
 "mkAdRyt"
);

 GAME.UI.addImage(
  "68852",
  "mkAdRytAvatar",
  true,
  "position:absolute;left:50%;top:0;transform:translateX(-50%);width:100px;height:100px;box-sizing:border-box;border-radius:10px;cursor:pointer;pointer-events:auto;",
  "mkAdRytBox"
 );

 crtDIV("mkAdRytName",true,
 "position:absolute;left:20px;top:120px;width:280px;height:24px;box-sizing:border-box;"+fnC+fnS1+ff+txAlgCen+"line-height:24px;cursor:pointer;pointer-events:auto;",
 "mkAdRytBox"
 );
 updDIVTxt("mkAdRytName",pNm);
 
 if (prefix == "pl") {adActNTools();}
 if (prefix == "mt") {adMtAct();}
 if (prefix == "bn") {adBnAct();}
}

# right panel

# center panel

action clrAdCen() {
 GAME.UI.removeDIV("mkAdList");

 crtDIV("mkAdList",true,
  "position:absolute;left:0;top:20px;width:580px;height:580px;box-sizing:border-box;padding:0 20px;overflow-y:auto;overflow-x:hidden;",
  "mkAdCen"
 );

 crtDIV("mkAdEmpty",true,
  "width:540px;height:60px;box-sizing:border-box;"+fnC+"font-size:18px;"+txAlgCen+"line-height:60px;",
  "mkAdList"
 );

 updDIVTxt("mkAdEmpty","CURRENTLY EMPTY");
}

action crtAdList() {
 crtDIV("mkAdList",true,
  "position:absolute;left:0;top:20px;width:580px;height:580px;box-sizing:border-box;padding:0 20px;overflow-y:auto;overflow-x:hidden;",
  "mkAdCen"
 );
}

action updAdList(str[] data, str prefix) {
 GAME.UI.removeDIV("mkAdList");

 crtDIV("mkAdList",true,
  "position:absolute;left:0;top:20px;width:580px;height:580px;box-sizing:border-box;padding:0 20px;overflow-y:auto;overflow-x:hidden;",
  "mkAdCen"
 );

 if (lengthOf data == 0) {
  crtDIV("mkAdEmpty",true,
   "width:540px;height:60px;box-sizing:border-box;"+fnC+"font-size:18px;"+txAlgCen+"line-height:60px;",
   "mkAdList"
  );
  updDIVTxt("mkAdEmpty","CURRENTLY EMPTY");
  return;
 }

 for (num i = 0; i < lengthOf data; i++) {
  str rowId = "mkAdRow_" + prefix + "_" + toStr(i);
  str nameId = "mkAdName_" + prefix + "_" + toStr(i);
  str actionId = "mkAdAction_" + prefix + "_" + toStr(i);

  str rowBg = adNorRowBg;
  str rowBrd = adNorRowBrd;

  if (data[i] == adSelPlr) {
   rowBg = adSelRowBg;
   rowBrd = adSelRowBrd;
  }

  crtDIV(rowId,true,
   "position:relative;width:540px;height:60px;margin-bottom:" + toStr(i < lengthOf data - 1 ? 10 : 0) + "px;box-sizing:border-box;background:" + rowBg + ";border:" + rowBrd + ";"+brdRad,
   "mkAdList"
  );

  crtDIV(nameId,true,
   "position:absolute;left:20px;top:0;width:380px;height:60px;box-sizing:border-box;"+fnC+fnS1+"text-align:left;line-height:60px;pointer-events:none;",
   rowId
  );

  updDIVTxt(nameId,data[i]);

  crtDIV(actionId,true,
   "position:absolute;right:10px;top:8.4px;width:100px;height:40px;box-sizing:border-box;background:rgba(190,190,190,1);"+brd+brdRad+fnC1+fnS2+txAlgCen+"line-height:42px;cursor:pointer;pointer-events:auto;",
   rowId
  );

  updDIVTxt(actionId,"ACTIONS");

  if (data[i] == adSelPlr) {
   updDIV(actionId,"background","rgba(255,255,255,1)");
  }
 }
}
bool action procAdListAct(str id,str[] data,str prefix) {
 for (num i = 0; i < lengthOf data; i++) {
  if (id == "mkAdAction_" + prefix + "_" + toStr(i)) {
   if (adSelPlr != "") {
    for (num j = 0; j < lengthOf data; j++) {
     if (data[j] == adSelPlr) {
      updDIV("mkAdRow_" + prefix + "_" + toStr(j),"border",adNorRowBrd);
      updDIV("mkAdRow_" + prefix + "_" + toStr(j),"background",adNorRowBg);
      updDIV("mkAdAction_" + prefix + "_" + toStr(j),"background","rgba(190,190,190,1)");
      break;
     }
    }
   }

   clrAdRytSel();

   adSelPlr = data[i];

   updDIV("mkAdRow_" + prefix + "_" + toStr(i),"border",adSelRowBrd);
   updDIV("mkAdRow_" + prefix + "_" + toStr(i),"background",adSelRowBg);
   updDIV("mkAdAction_" + prefix + "_" + toStr(i),"background","rgba(255,255,255,1)");

   updAdRytPlr(adSelPlr,prefix);
   return true;
  }
 }

 return false;
}
# center panel

# global/shared
action procDtRec(str id,obj data) {
 str[] ls = str[];
 str prefix = "";

 if (id=="plL") {ls=plrLs;prefix="pl";}
 else if (id=="mtL") {ls=mtLs;prefix="mt";}
 else if (id=="bnL") {ls=bnLs;prefix="bn";}
 else {return;}

 if ((str)data.d!="") {
  ls=(str[])data.d;

  if (id=="plL") {plrLs=ls;}
  if (id=="mtL") {mtLs=ls;}
  if (id=="bnL") {bnLs=ls;}
 }
 updAdList(ls,prefix);
}

action procPlrUpd(str id, obj data) {
 str pNm = (str)data.n;
 if (id == "plA") {addTo plrLs pNm;}
 if (id == "bnA") {addTo mtLs pNm;}
 if (id == "mtA") {addTo bnLs pNm;}
 if (id == "plD") {
  for (num i = lengthOf plrLs - 1; i >= 0; i--) {
   if (plrLs[i] == pNm) {remove plrLs[i];break;}
  }
 }
 if (adSelLst == "pl") {updAdList(plrLs,"pl");}
}

# global/shared

# base
action assgAdUI(obj d) {adminButtonIDs=(str[])d.b;adminButtonLabels=(str[])d.c;toolIDs=(str[])d.t;toolLabels=(str[])d.u;lmgs=(str[])d.l;smgs=(str[])d.s;rifles=(str[])d.r;
 launchers=(str[])d.x;pistols=(str[])d.p;shotguns=(str[])d.h;special=(str[])d.z;tools=(str[])d.w;
 num buttonTop=15;
 num buttonGap=70;

 for (num i=0;i<lengthOf adminButtonIDs;i++) {
  crtAdBtn(adminButtonIDs[i],adminButtonLabels[i],buttonTop+(i*buttonGap));
 }
}


action crtAdmPnl() {
 crtDIV("mkAdExt",true,"display:none;position:fixed;left:0;top:0;width:100%;height:100%;background:rgba(0,0,0,0);cursor:default;"+z,"");
 crtDIV("mkAdPnl",true,"display:none;position:fixed;left:50%;top:50%;transform:translate(-50%,-50%);width:1280px;height:720px;"+bg+brd+brdRad1+bx+z,"");

 crtDIV("mkAdTtl",true,ps + "left:0;top:28px;width:1280px;height:32px;"+txAlgCen+"color:" + txC+fnS+"pointer-events:none;","mkAdPnl");
 updDIVTxt("mkAdTtl","MK ADMIN PANEL");

 str[] id = str["mkAdLft","mkAdCen","mkAdRyt"];
 num[] x = num[20,340,940];
 num[] w = num[300,580,320];

 for (num i = 0; i < lengthOf id; i++) {
  crtDIV(id[i],true,st + "left:" + toStr(x[i]) + "px;top:80px;width:" + toStr(w[i]) + "px;height:620px;"+bg1+brd+cr, "mkAdPnl");
 }
 crtDIV("mkAdCls",true,"position:absolute;right:20px;top:18px;width:40px;height:40px;"+fnC+fnS+txAlgCen+
 "line-height:38px;cursor:pointer;box-sizing:border-box;","mkAdPnl");

 updDIVTxt("mkAdCls", "X");
}

action clrAdSel() {
 if (adSelBtn != "") {
  updDIV(adSelBtn,"border",norBrd);
  updDIV(adSelBtn,"background",btnBg);
 }

 if (adSelRyt != "") {
  updDIV(adSelRyt,"border",norBrd);
  if (adSelRyt == "mkAdRytBan") {updDIVTxt(adSelRyt,"BAN");}
 }

 if (adSelWep != "") {
  updDIV(adSelWep,"border",norBrd);
 }

 adSelBtn = "";
 adSelPlr = "";
 adSelRyt = "";
 adSelWep = "";

 clrAdCen();
 clrAdRyt();
}
action clsAdPnl() {
 adminPanelOpen = false;
 updDIV("mkAdExt", "display", "none");
 updDIV("mkAdPnl", "display", "none");
 GAME.INPUTS.lockMouse();
 clrAdSel();
}
action toggAdPan() {
 if (adminPanelOpen) {
  clsAdPnl();
 } else {
  adminPanelOpen = true;
  updDIV("mkAdExt", "display", "block");
  updDIV("mkAdPnl", "display", "block");
  GAME.INPUTS.unlockMouse();
 }
}
#base

# -ADMIN SYSTEM-

# Runs when the game starts
public action start() {
crtAdmPnl();
crtAdList();
crtAdRyt();
}

# Runs every game tick
public action update(num delta) {

}

# Add rendering logic in here
public action render(num delta) {

}

# Player spawns in
public action onPlayerSpawn(str id) {

}

# Player died
public action onPlayerDeath(str id, str killerID) {

}

# Player update
public action onPlayerUpdate(str id, num delta, obj inputs) {

}

# User pressed a key
public action onKeyPress(str key, num code) {
 if (key == "m" && k != "") {toggAdPan();clrAdSel();}
}

# User released a key
public action onKeyUp(str key, num code) {

}

# User held a key
public action onKeyHeld(str key, num code) {

}

# User pressed a button on a controller
public action onControllerPress(str key, num code) {

}

# User released a button on a controller
public action onControllerUp(str key, num code) {

}

# User held a button on a controller
public action onControllerHeld(str key, num code) {

}

# User clicked on screen
public action onMouseClick(num button, num x, num y) {

}

# User released clicked on screen
public action onMouseUp(num button, num x, num y) {

}

# User scrolled on screen
public action onMouseScroll(num dir) {

}

# User clicked a DIV (ID)
public action onDIVClicked(str id) {
if (id == "mkAdCls") {clsAdPnl();return;}
if (id == "mkAdExt") {clsAdPnl();return;}
if (id == "mkAdRytAvatar" || id == "mkAdRytName") {GAME.UTILS.copyToClipboard(adSelPlr);return;}
if (procAdListAct(id,plrLs,"pl")) {return;}
if (procAdListAct(id,mtLs,"mt")) {return;}
if (procAdListAct(id,bnLs,"bn")) {return;}

if (id == "mkAdBtnPlrs") {selAdBtn(id);adSelLst="pl";adSelPlr="";clrAdRyt();netSd("bPl",{sI:k,r:"rq"});return;}
if (id == "mkAdBtnMt") {selAdBtn(id);adSelLst="mt";adSelPlr="";clrAdRyt();netSd("bMt",{sI:k,r:"rq"});return;}
if (id == "mkAdBtnBn") {selAdBtn(id);adSelLst="bn";adSelPlr="";clrAdRyt();netSd("bBn",{sI:k,r:"rq"});return;}
#if (id == "mkAdBtnVp") {selAdBtn(id);netSd("bPl", {sI:k,r: "rq"});return;}
#if (id == "mkAdBtnAd") {selAdBtn(id);netSd("bPl", {sI:k,r: "rq"});return;}
#if (id == "mkAdBtnCon") {selAdBtn(id);netSd("bPl", {sI:k,r: "rq"});return;}
#if (id == "mkAdBtnOth") {selAdBtn(id);netSd("bPl", {sI:k,r: "rq"});return;}

procAdRytAct(id);
procAdWep(id);

}

# Client receives network message
public action onNetworkMessage(str id, obj data) {
 if (id == "sID") {assgSess(data);}
 if (id=="aUI") {assgAdUI(data);}
 if (id == "lS") {logS(data);}
 if (id == "plA" || id == "plD") {procPlrUpd(id,data);return;}
 if (id == "bnA"|| id == "mtA") {procPlrUpd(id,data);return;}
 procDtRec(id, data);
}