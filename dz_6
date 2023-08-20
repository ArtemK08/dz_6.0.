import sys, os

GROUPS = {"audio": {'MP3', 'OGG', 'WAV', 'AMR'},
          "images": {'JPEG', 'PNG', 'JPG', 'SVG', 'BMP'},
          "video": {'AVI', 'MP4', 'MOV', 'MKV'},
          "documents": {'DOC', 'DOCX', 'TXT', 'PDF', 'XLSX', 'PPTX'},
          "archives": {'ZIP', 'GZ', 'TAR', 'RAR'}
          }

known_files = { key: set() for key in GROUPS }
unknown_files = set()
all_ext = set()
known_ext = set()
unknown_ext = set()
ext2key = {}
for key, set_of_ext in GROUPS.items():
    all_ext.update(set_of_ext)
    for ext in set_of_ext:
        ext2key[ext]=key

def check_groups(dir):
    root = os.listdir(dir)
    for name in GROUPS:
        if name not in root:
            os.mkdir(dir + "\\" + name, 0)


def normalize(name):

    tran={'.':'.','q':'q','w':'w','1':'1','2':'2','3':'3','4':'4','5':'5','6':'6','7':'7','8':'8','9':'9','0':'0',
        't':'t','b':'b','m':'m','p':'p','x':'x','ш':'sh','л':'l','р':'r','п':'p','с':'s','C':'S','B':'B','т':'t',
        'Т':'T','щ':'shch','Ш':'SH','ч':'ch','П':'P','M':'M','P':'P','U':'U','N':'N','n':'n','u':'u','н':'n','Н':'N',
        'Щ':'SHCH','X':'X','Ч':'CH','T':'T','E':'E','e':'e','е':'e','Q':'Q','W':'W','O':'O','о':'o','o':'o','О':'O',
         ':':':',',':',','б':'b','Б':'B','Л':'L','Д':'D','д':'d','d':'d','D':'D','K':'K','к':'k','k':'k','К':'K',
          'є':'ye','Є':'YE','ж':'zh','Ж':'ZH','и':'y','И':'Y','y':'y','Y':'Y','ye':'ye','YE':'YE','ZH':'ZH','а':'a',
          'А':'A','Ф':'F','ф':'f','f':'f','F':'F','a':'a','A':'A','і':'i','І':'I','I':'I','i':'i','й':'y','Й':'Y','г':'h',
          'Г':'H','s':'s','S':'S','я':'ya','Я':'YA','ya':'ya','YA':'YA','з':'z','З':'Z','Z':'Z','в':'v','В':'V',
          'v':'v','V':'V','х':'kh','Х':'KH','KH':'KH','kh':'kh','ю':'yu','ю':'YU','yu':'yu','YU':'YU','ц':'ts','Ц':'TS',
          'TS':'TS','ts':'ts','ї':'yi','Ї':'YI','yi':'yi','YI':'YI'}
    chars = [tran[ch] if ch in tran else "_" for ch in name]
    return "".join(chars)

def clear_empty(dir):
    for record in os.walk(dir):
        if record[1]==[] and record[2]==[]:
            os.rmdir(record[0])

def move_files(dir):
    check_groups(dir)
    for record in os.walk(dir):
        if record[0] in {dir + "\\" + key for key in GROUPS}: continue
        for file in record[2]:
            ext = file.split('.')[-1].upper() if "." in file else ""
            name=record[0]+"\\"+file
            norm_name=normalize(name)
            if ext in all_ext:
                os.rename(name, dir+"\\"+ext2key[ext]+"\\"+normalize(file))
            else:
                if name!=norm_name:
                    os.rename(name, norm_name)

    clear_empty(dir)


def dir_process(dir):
    for record in os.walk(dir):
        if record[0] in {dir + "\\" + key for key in GROUPS}: continue
        for f in record[2]:
            ext = f.split('.')[-1].upper() if "." in f else ""
            if ext in all_ext:
                known_ext.add(ext)
                known_files[ext2key[ext]].add(f)
            else:
                unknown_ext.add(ext)
                unknown_files.add(f)
    print("Known extention:", *known_ext)
    print("Unknown extention:", *unknown_ext)
    for key in known_files:
        print(f"Category {key}:", *known_files[key])
    print("Without category:", *unknown_ext)

    move_files(dir)

dir_process(sys.argv[-1])
