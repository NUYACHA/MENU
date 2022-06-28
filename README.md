function MENU1()

A = gg.choice ({

	"รวมสคริปต์",

	"แปลภาษา",

    "กลับ",

},nil,"ใช้งานทั่วไป")

if A == nil then else

if A==1 then A1() end

if A==2 then MODE() end

if A==3 then os.exet() end

end

NUX=-1

end

function MODE()

TRANSLATE=nil

MENU = gg.choice({

   "ภาษาTH-->ภาษาอังEN",

   "ภาษาอังEN-->ภาษาTH",

   "ออก",

},nil,"เลือกภาษาที่จะแปล")

if MENU == nil then else

if MENU == 1 then TH() end

if MENU == 2 then EN() end

if MENU == 3  then os.exit() end

end

NUX=-1

end

function TH()

TRANSLATE=gg.prompt({"ภาษาTH-->ภาษาอังEN"}, nil, {"text"}) 

if TRANSLATE==nil then

gg.alert("ไม่ได้ใส่คำแปล")

MODE()

end

LANGUAGE="th"

LANGUAGE1="en"

MT()

end

function EN()

TRANSLATE=gg.prompt({"ภาษาอังEN-->ภาษาTH"}, nil, {"text"}) 

if TRANSLATE==nil then

gg.alert("ไม่ได้ใส่คำแปล")

MODE()

end

LANGUAGE="en"

LANGUAGE1="th"

MT()

end

function MT()

TRANSLATE = gg.makeRequest("https://translate.googleapis.com/translate_a/single?client=gtx&sl="..LANGUAGE.."&tl="..LANGUAGE1.."&dt=t&q="..TRANSLATE[1], {['User-Agent']="Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36"}).content

TRANSLATE1 = {} for _ in TRANSLATE:gmatch("\"(.-)\"") do TRANSLATE1[#TRANSLATE1 + 1] = _ end

LOLI = gg.alert(TRANSLATE1[1],"กลับ","คัดลอกคำแปล")

if LOLI == nil then else

if LOLI == 1 then MODE() end

if LOLI == 2 then gg.copyText(TRANSLATE1[1]) end

end

end

function A1()

local gg, die = gg, os.exit

local slowdown = function () gg.sleep(500) end

local configFile = gg.EXT_FILES_DIR .. '/smart_loader_save.lua.cfg'

local scripts

local loaderMenu = {}

local alert

function LoadSavedList()

	debug = -1

	slowdown()

	scripts = {['name'] = {}, ['path'] = {}}

	local data = loadfile(configFile)

	if data ~= nil then

		data = data()

		if (type(data)) == 'table' then

			for k, v in ipairs (data.path) do

				if not loadfile(v) then

					table.remove(data.path, k)

					table.remove(data.name, k)

				end

			end

			scripts = data

			gg.saveVariable(scripts, configFile)

			gg.alert(#scripts.path .. ' สคริปต์ในลิสต์')

		end

	end

end

LoadSavedList()

function AddScript()

	local ask = gg.prompt({

		"[ 📁 ] เลือกไฟล์อัพโหลด :\n➣",

	},{'/sdcard'},{'file'})

	if ask == nil then return nil end

	

	local split = {}

	local path = ask[1] .. '/'

	for str in path:gmatch '.-/' do

		str = str:gsub('/', '')

		table.insert(split, str)

	end

	if not (loadfile(ask[1])) or not (split[#split]:match '.lua') then

		gg.alert('`' .. split[#split] .. '` ไม่สามารถเลือกไฟล์ได้')

		return nil

	end

	table.insert(scripts.path, ask[1])

	table.insert(scripts.name, split[#split])

	gg.saveVariable(scripts, configFile)

end

function RemoveScript()

	debug = -1

	slowdown()

	

	while true do

		local removeMenu = {}

		for i=1, #scripts.name do

			local name = table.unpack(scripts.name, i)

			removeMenu[i] = '[ 🗑️ ] ' .. name

		end

		table.insert(removeMenu, '[ ↪️ ] กลับไป')

		

		local description = #removeMenu > 1 and "" or "ไม่มีสคริปต์ให้ลบ" 

		local menu = gg.choice(removeMenu, 0, description)

		if menu == #removeMenu or menu == nil then

			break

		end

		alert = gg.alert('ลบ `' .. scripts.name[menu] .. '` ?', 'ไม่', 'ใช่')

		if alert == 2 then

			table.remove(scripts.path, menu)

			table.remove(scripts.name, menu)

			gg.saveVariable(scripts, configFile)

			LoadSavedList()

		end

	end

end

function ClearConfig()

	debug = -1

	slowdown()

	alert = gg.alert('คุณแน่ใจใช่ไหมที่จะลบที่อยู่ไฟล์ทั้งหมด ?', 'ไม่', 'ใช่')

	if alert == 2 then

		slowdown()

		local f = io.open(configFile, 'w')

		f:close()

		gg.alert('ลบที่อยู่ไฟล์ทั้งหมด 🗑️')

		LoadSavedList()

	end

end

function Settings()

	debug = -1

	slowdown()

	local menu = gg.choice({

		"[ 📂 ] นำเข้าสคริปต์",

		"[ ⛔ ] ลบ",

		"[ 🗑️ ] ลบทั้งหมด",

		"[ ⬅️ ] กลับ"

	}, 0, "")

	if menu == nil then return nil end

	if menu == 1 then AddScript() end

	if menu == 2 then RemoveScript() end

	if menu == 3 then ClearConfig() end

	if menu == 4 then die() end

end

function UpdateMenu()

	debug = -1

	loaderMenu = {}

	if #scripts.name > 0 then

		for i=1, #scripts.name do

			local name = table.unpack(scripts.name, i)

			loaderMenu[i] = '[ ' .. i .. ' ] ' .. name .. '\n'

		end

	end

	table.insert(loaderMenu, "[ ⚙️ ] ตั้งค่า")

end

function Loader()

	debug = -1

	UpdateMenu()

	local description = #loaderMenu > 1 and "" or "ไม่มีสคริปต์ที่บันทึกไว้"

	local menu = gg.choice(loaderMenu, 0, description)

	if menu == nil then return nil end

	if menu == #loaderMenu then

		Settings(); return nil

	end

	gg.setVisible(true)

	pcall(loadfile(scripts.path[menu]))

end

while true do

	if gg.isVisible(true) then

		gg.setVisible(false) debug = 1

	end

	if debug == 1 then Loader() end

end

end

while true do

if gg.isVisible(true) then

NUX=1

gg.setVisible(false)

end

if NUX==1 then MENU1()

NUX=-1

end

end
