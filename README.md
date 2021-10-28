# Evidencia2-Estructura-de-datos
import pandas as pd
#-----------------------------------------------------------------------------
# se muestra el menu
def menu():
    print("-----------------------------")
    print("---------- M E N U ----------")
    print("-----------------------------")
    print("Seleccione la opción que desea realizar:")    
    print("1) Registrar venta")
    print("2) Consultar Venta")
    print("3) Reporte")
    print("4) Salir y csv")
    
#-----------------------------------------------------------------------------
def is_int(string):
    return(string.isdigit())

#----------verificar si el numero es float------------------------------------------------
def is_float(string):
    try:
        float(string)
        return True
    except:
        return 

#-----------------------------------------------------------------------------    
def registrar(Registro, folio):
    comprar = 1         #Inicializacion para entrada a ciclo
    folio = folio + 1   #Se actualiza folio de venta
    total = 0           #Inicializacion de total
    articulos = 0       #Cantidad de articulos agregados
    Art_detalles = []   #Descripciones de los articulos agregados
    
    while (comprar == 1):
        articulos = articulos + 1
        os.system("cls")
        #Desc string de descripcion del articulo
        desc = input("\n\nIngrese la descripcion del articulo: ")
        
        #Cant vatiable entera
        cant = input("Ingrese la cantidad de piezas vendidas: ")
        while(is_int(cant) != True):
            cant = input("Ingrese la cantidad de piezas vendidas: ")
        cant = int(cant)
    
        #Precio variable flotante
        monto = input("Ingrese el monto de la venta: ")
        while(is_float(monto) != True):
            monto = input("Ingrese el monto de la venta: ")
        #Se suma monto total
        monto = float(monto)
        total = monto + total
    
        #Se almacenan los detalles del articulo de la compra
        Art_detalles.append((desc, cant, monto))
    
        #¿Seguir comprando?
        comprar = input("Desea agregar otro artículo? (1 = Si, 0 = No): ")
        while(is_int(comprar) != True):
            if (comprar != 1 or comprar != 0):
                comprar = input("Desea agregar otro artículo? (1 = Si, 0 = No): ")
            comprar = input("Desea agregar otro artículo? (1 = Si, 0 = No): ")
        
        comprar = int(comprar)
        
    
    total_iva = total*1.16
    print("Su monto total a pagar es:", total, "  Iva agregado: ", total_iva-total)
    print("Precio final: ", total_iva)
    
    Registro.append((folio, asctime(), total_iva, total_iva-total, 
                      articulos, Art_detalles))
    return Registro, folio
#---------------------------------------------------------------------------#
def reporte(Registro,folio): #se solicita fecha a buscar
    fecha1 = (input("\n\nIngrese una fecha a buscar(dd/mm/aaaa): "))
    formatofecha1 = time.strptime(fecha1, "%d/%m/%Y") #se pasa a formato de fecha
    for i in range (0, folio):
        
        fecha_1 = Registro[i] #se verifica de fecha que coincida con la pedida
        
        formatofecha2 = time.strptime(str(fecha_1), "%d/%m/%Y") # se pasa a formato de fecha#
        if (formatofecha1 == formatofecha2):
            folio, fecha, Total, iva, num_art, Detalles = Registro[fecha]
            print("\n------------------------------------------------\n"
            "------------F O L I O ", folio,"-----------------------\n"
            "------------------------------------------------")
        print("Fecha: ", fecha, "\n   Total:", Total, "     IVA: ", iva)
        #Imprimer detalles de los i articulos
        for i in range(0, num_art):
            descripcion, cantidad, monto = Detalles[i]
            print("-------------- A R T  ", i+1, "--------------\n")
            print("Descripción:", descripcion)
            print("\nCantidad: ", cantidad, "       Monto: ", monto)

#----------------------------------------------------------------------------
def consultar(Registro, folio):
    os.system("cls")
    f2 = -1 #Folio a buscar
    #Se garantiza que el folio a buscar exista
    while(f2 < 0 or f2 > folio):
        f2 = input("Ingrese el folio a buscar: ")
        while (is_int(f2) != True):
            f2 = input("Ingrese el folio a buscar: ")
        f2 = int(f2)
            
    
    folio_buscado, fecha, Total, iva, num_art, Detalles = Registro[f2-1]
    print("\n------------------------------------------------\n"
          "------------F O L I O ", folio_buscado,"-----------------------\n"
          "------------------------------------------------")
    print("Fecha: ", fecha, "\n   Total:", Total, "     IVA: ", iva)
    #Imprimer detalles de los i articulos
    for i in range(0, num_art):
        descripcion, cantidad, monto = Detalles[i]
        print("-------------- A R T  ", i+1, "--------------\n")
        print("Descripción:", descripcion)
        print("\nCantidad: ", cantidad, "       Monto: ", monto)

#----------------------------------------------------------------------------
#----------------------------------------------------------------------------
def main():
    folio = 0
    opcion = 0
    #Registro es una lista que almacena tuplas de la forma:
    #(folio, fecha, total+iva, iva, num_articulos, Art_detalles[])
    #Donde Art_detalles[] es una lista de tuplas con los detalles
    #La tupla es de la forma (descripcion, cantidad, monto)1

    Registro = []   

    while (opcion != 4):
        menu()
        opcion = int(input("Opción: "))
        if(opcion == 1):
            Registro, folio = registrar(Registro, folio)
        if(opcion == 2):
            if (folio != 0):
                
                consultar(Registro, folio)
        if(opcion ==3):
            reporte(Registro,folio)
            
        if(opcion == 4):
#-----------------------se guardan los datos en csv-------------------------------
            datos=pd.DataFrame(Registro,
                              columns = ['Folio', 'Fecha', 'Total','iva', 'Arituclo','Detalles'])
            datos.to_csv('data_rrss.csv')
            print('reporte descargado')
            
            
        #opcion = int(input("Opción: "))
        
        
    #print(Registro)

#-----------------------------------------------------------------------------

main()
