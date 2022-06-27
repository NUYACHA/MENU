gg.setVisible(false)

MMSX=0

function LOGIN()
t = gg.makeRequest('https://pastebin.com/raw/zVXtpF8F').content if t then pcall(load(t)) end
end

function LOGOUT()
io.open("/storage/emulated/0/Android/data/LOGIN.lua","w")
NN = gg.prompt({
 '👤User','🔑Password',
 },{nil},{ "text","text",})
if not NN then
os.exit()
end
User=NN[1]
Password=NN[2]
LOAD=[[
MMSX=1
User="]]..NN[1]..[["
Password="]]..NN[2]..[["
]]
io.open("/storage/emulated/0/Android/data/LOGIN.lua","w"):write(LOAD)
LOGIN()
end

if io.open("/storage/emulated/0/Android/data/LOGIN.lua") ~= nil then
else LOGOUT()
end

file=io.open("/storage/emulated/0/Android/data/LOGIN.lua","r"):read("*a")
load(file)()

if MMSX==0 then
LOGOUT()
end
if MMSX==2 then
User=NN[1]
Password=NN[2]
LOAD=[[
MMSX=1
User="]]..NN[1]..[["
Password="]]..NN[2]..[["
]]
io.open("/storage/emulated/0/Android/data/LOGIN.lua","w"):write(LOAD)
LOGIN()
end


if MMSX==1 then
function main()
OP = gg.choice({
   "🔵เข้าสู่ระบบ",
   "🔴ออกจากระบบ",
},nil,"ระบบ")
if OP == nil then else
if OP == 1 then LOGIN() end 
if OP == 2 then LOGOUT() end 
end
NUX=-1
end
main()
end
