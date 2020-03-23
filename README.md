# esx_vehicleshop

## esx_vehocleshop\client\main.lua
* add in line 309 ~ 379
```lua 
ESX.UI.Menu.Open('default', GetCurrentResourceName(), 'shop_money', {
    title = _U('chose_money'),
    align = 'top-left',
    elements = {
        {label = _U('money_type'),   value = 'money'},
        {label = _U('black_type'), value = 'black'}
}}, function (data3, menu3)
    if data3.current.value == 'money' then
        ESX.TriggerServerCallback('esx_vehicleshop:buyVehicle', function (hasEnoughMoney)
            if hasEnoughMoney then
                IsInShopMenu = false
                menu3.close()
                menu2.close()
                menu.close()
                DeleteShopInsideVehicles()

                ESX.Game.SpawnVehicle(vehicleData.model, Config.Zones.ShopOutside.Pos, Config.Zones.ShopOutside.Heading, function (vehicle)
                    TaskWarpPedIntoVehicle(playerPed, vehicle, -1)

                    local newPlate     = GeneratePlate()
                    local vehicleProps = ESX.Game.GetVehicleProperties(vehicle)
                    vehicleProps.plate = newPlate
                    SetVehicleNumberPlateText(vehicle, newPlate)

                    if Config.EnableOwnedVehicles then
                        TriggerServerEvent('esx_vehicleshop:setVehicleOwned', vehicleProps)
                    end

                    ESX.ShowNotification(_U('vehicle_purchased'))
                end)

                FreezeEntityPosition(playerPed, false)
                SetEntityVisible(playerPed, true)
            else
                ESX.ShowNotification(_U('not_enough_money'))
            end
        end, vehicleData.model)
    elseif data3.current.value == 'black' then
        ESX.TriggerServerCallback('esx_vehicleshop:buyVehicleblack', function (hasEnoughMoney)
            if hasEnoughMoney then
                IsInShopMenu = false
                menu3.close()
                menu2.close()
                menu.close()
                DeleteShopInsideVehicles()

                ESX.Game.SpawnVehicle(vehicleData.model, Config.Zones.ShopOutside.Pos, Config.Zones.ShopOutside.Heading, function (vehicle)
                    TaskWarpPedIntoVehicle(playerPed, vehicle, -1)

                    local newPlate     = GeneratePlate()
                    local vehicleProps = ESX.Game.GetVehicleProperties(vehicle)
                    vehicleProps.plate = newPlate
                    SetVehicleNumberPlateText(vehicle, newPlate)

                    if Config.EnableOwnedVehicles then
                        TriggerServerEvent('esx_vehicleshop:setVehicleOwned', vehicleProps)
                    end

                    ESX.ShowNotification(_U('vehicle_purchased'))
                end)

                FreezeEntityPosition(playerPed, false)
                SetEntityVisible(playerPed, true)
            else
                ESX.ShowNotification(_U('not_enough_money'))
            end
        end, vehicleData.model)
    end
end,function (data3, menu3)
menu3.close()
end)
```

## esx_vehicle\server\main.lua
* add in line 230 ~ 248
```lua
ESX.RegisterServerCallback('esx_vehicleshop:buyVehicleblack', function (source, cb, vehicleModel)
	local xPlayer     = ESX.GetPlayerFromId(source)
	local AccountBlack = xPlayer.getAccount('black_money')
	local vehicleData = nil

	for i=1, #Vehicles, 1 do
		if Vehicles[i].model == vehicleModel then
			vehicleData = Vehicles[i]
			break
		end
	end

	if AccountBlack.money >= vehicleData.price then
		xPlayer.removeAccountMoney('black_money', vehicleData.price)
		cb(true)
	else
		cb(false)
	end
end)
```

## esx_vehicles\locles\en.lua
* add in line 67 ~ 70
```lua
  --Money type
  ['chose_money']             = '選擇支付選項',
  ['money_type']              = '現金',
  ['black_type']              = '黑錢',
```
