з  tkinter  import  Tk , Canvas
 випадковий імпорт

# Глобальні
ШИРИНА  =  800
ВИСОТА  =  600
SEG_SIZE  =  20
IN_GAME  =  Правда


# Допоміжні функції
def  create_block ():
    """ Створює яблуко, яке потрібно з'їсти """
    глобальний  БЛОК
    posx  =  SEG_SIZE  *  випадковий . randint ( 1 , ( ШИРИНА - SEG_SIZE ) /  SEG_SIZE )
    posy  =  SEG_SIZE  *  випадковий . randint ( 1 , ( ВИСОТА - SEG_SIZE ) /  SEG_SIZE )
    БЛОКУВАТИ  =  c . create_oval ( posx , posy ,
                          posx + SEG_SIZE , posy + SEG_SIZE ,
                          fill = "червоний" )


def  main ():
    """ Керує процесом гри """
    глобальна  IN_GAME
    якщо  IN_GAME :
        s . рухатися ()
        head_coords  =  c . координати ( s . сегменти [ - 1 ]. екземпляр )
        x1 , y1 , x2 , y2  =  координати голови
        # Перевірка на зіткнення з краями ігрового поля
        якщо  x2  >  WIDTH  або  x1  <  0  або  y1  <  0  або  y2  >  HEIGHT :
            IN_GAME  =  False
        # Їсти яблука
        elif  head_coords  ==  c . координати ( БЛОК ):
            s . add_segment ()
            c . видалити ( БЛОКУВАТИ )
            create_block ()
        # Самоїда
        ще :
            для  індексу  в  діапазоні ( len ( s . segments ) - 1 ):
                if  head_coords  ==  c . coords ( s . segments [ index ]. instance ):
                    IN_GAME  =  False
        корінь . після ( 100 , основний )
    # Not IN_GAME -> зупинити гру та надрукувати повідомлення
    ще :
        set_state ( restart_text , 'normal' )
        set_state ( game_over_text , 'normal' )


клас  Сегмент ( об'єкт ):
    """ Один сегмент змії """
    def  __init__ ( self , x , y ):
        себе . екземпляр  =  c . create_rectangle ( x , y ,
                                           x + SEG_SIZE , y + SEG_SIZE ,
                                           fill = "білий" )


клас  Snake ( об'єкт ):
    """ Клас проста змія """
    def  __init__ ( self , segments ):
        себе . сегменти  =  сегменти
        # можливих ходів
        себе . mapping  = { "Вниз" : ( 0 , 1 ), "Вправо" : ( 1 , 0 ),
                        "Вгору" : ( 0 , - 1 ), "Ліворуч" : ( - 1 , 0 )}
        # початковий напрямок руху
        себе . вектор  =  себе . відображення [ "Вправо" ]

    def  move ( self ):
        """ Переміщує змійку з указаним вектором"""
        для  індексу  в  діапазоні ( len ( self . segments ) - 1 ):
            сегмент  =  self . сегменти [ індекс ]. екземпляр
            x1 , y1 , x2 , y2  =  c . coords ( self . segments [ index + 1 ]. instance )
            c . координати ( сегмент , x1 , y1 , x2 , y2 )

        x1 , y1 , x2 , y2  =  c . coords ( self . segments [ - 2 ]. instance )
        c . coords ( self . segments [ - 1 ]. екземпляр ,
                 x1 + себе . вектор [ 0 ] * SEG_SIZE , y1 + self . вектор [ 1 ] * SEG_SIZE ,
                 x2 + себе . вектор [ 0 ] * SEG_SIZE , y2 + self . вектор [ 1 ] * SEG_SIZE )

    def  add_segment ( self ):
        """ Додає сегмент до змійки """
        last_seg  =  c . coords ( self . segments [ 0 ]. instance )
        x  =  last_seg [ 2 ] -  SEG_SIZE
        y  =  last_seg [ 3 ] -  SEG_SIZE
        себе . сегменти . вставити ( 0 , відрізок ( x , y ))

    def  change_direction ( self , event ):
        """ Змінює напрямок змії """
        якщо  подія . keysym  в  себе . відображення :
            себе . вектор  =  себе . відображення [ подія . keysym ]

    def  reset_snake ( self ):
        для  сегмента  в  себе . сегменти :
            c . видалити ( сегмент . екземпляр )


def  set_state ( елемент , стан ):
    c . itemconfigure ( елемент , стан = стан )


def  клацнув ( подія ):
    глобальна  IN_GAME
    s . reset_snake ()
    IN_GAME  =  Правда
    c . видалити ( БЛОКУВАТИ )
    c . itemconfigure ( restart_text , state = 'hidden' )
    c . itemconfigure ( game_over_text , state = 'hidden' )
    початок_гри ()


def  start_game ():
    глобальний  s
    create_block ()
    s  =  create_snake ()
    # Реакція на натискання клавіші
    c . bind ( "<KeyPress>" , s . change_direction )
    головний ()


def  create_snake ():
    # створення сегментів і змійки
    segments  = [ Сегмент ( SEG_SIZE , SEG_SIZE ),
                Сегмент ( SEG_SIZE * 2 , SEG_SIZE ),
                Сегмент ( SEG_SIZE * 3 , SEG_SIZE )]
    повернення  Змія ( сегменти )


# Вікно налаштування
корінь  =  Tk ()
корінь . назва ( "PythonicWay Snake" )


c  =  Canvas ( корінь , ширина = WIDTH , висота = HEIGHT , bg = "#003300" )
c . сітка ()
# ловити натискання клавіш
c . focus_set ()
game_over_text  =  c . create_text ( WIDTH / 2 , HEIGHT / 2 , text = "ГРУ ЗАВЕРШЕНО!" ,
                               font = 'Arial 20' , fill = 'red' ,
                               стан = 'прихований' )
restart_text  =  c . create_text ( WIDTH / 2 , HEIGHT - HEIGHT / 3 ,
                             font = 'Arial 30' ,
                             fill = 'білий' ,
                             text = "Натисніть тут, щоб перезапустити" ,
                             стан = 'прихований' )
c . tag_bind ( restart_text , "<Button-1>" , натиснуто )
початок_гри ()
корінь . основний цикл ()
