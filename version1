
import  requests
import re
import os
from urllib.request import urlretrieve
import multiprocessing
from tkinter import *
flag=0
def get_url():
    key = entry.get()
    url='https://xueshu.baidu.com/s?wd='+key+'&ie=utf-8&tn=SE_baiduxueshu_c1gjeupa&sc_from=&sc_as_para=sc_lib%3A&rsv_sug2=0'
    return url

headers={
        'user-agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.198 Safari/537.36',
        'Referer': 'https://googleads.g.doubleclick.net/'}

def get_paper_link():
    headers={'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.198 Safari/537.36'}
    #获取网页内容
    response=requests.get(url=get_url(),headers=headers)
    res1_data=response.text
    #print(res1_data)
    #找论文链接
    paper_link=re.findall(r'<h3 class=\"t c_font\">\n +\n +<a href=\"(.*)\"',res1_data)
    #print(paper_link)
    #补齐链接
    doi_list=[]#用一个列表接收论文的DOI
    for link in paper_link:
        paper_link='http:'+link
        #print(paper_link)
        response2=requests.get(url=paper_link,headers=headers)
        response2.encoding='GBK'#读取中文时不会出现乱码
        res2_data = response2.text
        #提取论文的DOI
        try:
            paper_doi=re.findall(r'\'doi\'}\">\n +(.*?)\n ',res2_data)
            if str(10) in  paper_doi[0]:
                #print(paper_doi)
                doi_list.append(paper_doi)

        except:
            pass
    return doi_list
def load_paper(title):


    text.insert(END,'论文：{}，正在下载...'.format(title))
    text.see(END)#文本框滚动
    text.update()#更新
    text.insert(END, '论文：{}，下载结束！'.format(title))
    text.see(END)
    text.update()

#构建sci-hub下载链接
def doi_download():
    flag=1
    doi_list=get_paper_link()
    #print(doi_list)
    for doi in doi_list:
        #print(doi[0])
        doi_link='https://sci.bban.top/pdf/'+doi[0]+'.pdf'

        print(doi_link)
        #paper_d=requests.get(url=doi_link,headers=headers).text
        #download_link=re.findall(r"href='(.*?)'",paper_d)
        #print(download_link)

        if 'https:' not in doi_link:
            doi_link='https:'+doi_link

        #print(doi_link)
        r=requests.get(url=doi_link,headers=headers)
        r.close()
        #print(r.content)
        paperpath = entry2.get()
        if  not os.path.exists(paperpath):
            os.mkdir(paperpath)
        path=paperpath+'//'+doi_link.split('/')[-1]
        with open(path,'wb') as f:
            f.write(r.content)

            print('下载完毕'+doi_link.split('/')[-1])
            load_paper(doi_link.split('/')[-1])
#paper_d=requests.get(url='https://sci-hub.tf/10.1109/5.628714',headers=headers).text
#print(paper_d)
#url='https://xueshu.baidu.com/s?wd=swot%E5%88%86%E6%9E%90&rsv_bp=0&tn=SE_baiduxueshu_c1gjeupa&rsv_spt=3&ie=utf-8&f=3&rsv_sug2=1&sc_f_para=sc_tasktype%3D%7BfirstSimpleSearch%7D&rsp=0'
#doi_download()
#print(get_paper_link(url))
#开发GUI页面
# 创建画布
root = Tk()
root.configure(background = 'white')
root.geometry('650x450')
bp=PhotoImage(file="C:/Users/DELL/Desktop/4.gif")
root.title('论文爬取器')
# 文本框
label = Label(root, text='关键字（仅限英文）：', font=('DFKai-SB', 15))
label.grid(row=0, column=0)
label2 = Label(root, text='输入路径：', font=('DFKai-SB', 15))
label2.grid(row=1, column=0)
# 输入框
entry = Entry(root, font=('DFKai-SB', 20))
entry.grid(row=0, column=1)
entry2 = Entry(root, font=('DFKai-SB', 20))
entry2.grid(row=1, column=1)
print(get_url())
# 列表框
text = Listbox(root, font=('隶书', 16), width=50, height=15)
text.grid(row=2, columnspan=2)
# 下载按钮
button1 = Button(root, image=bp,text='start', font=('MingLiU', 13),command=doi_download)
button1.grid(row=0, column=2,rowspan=2)
'''
if (flag==1):
    button1 = Button(root, text='end',image=bp, font=('MingLiU', 13), command=root.quit)
'''
root.mainloop()
