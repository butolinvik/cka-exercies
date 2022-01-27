### Здесь будут задания, ориентированные на повышенную сложность. А-ля killes.sh
<ul>
    <li>Подготовка</li>
</ul>
<H3>Подготовка</H3>
<code>
#Для экономии времени создаем алиасы kubectl и dry-run  

alias k=kubectl  
export do="--dry-run=client -oyaml"  
export now="--force --grace-period 0" # удалить под моментально  
#Редактируем ~/.vimrc, чтобф автоматический таб в vim вел себя нормально, а не делал адского размера пробелы  
set tabstop=2  
set expandtab  
set shiftwith=2  
</code>


