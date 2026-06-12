<script>

export default {
    data() {
        return {
            machine_types: [
                {
                    name: 'Smelter',
                    id: 'smelter',
                    color: '#D32F2F',
                    recipes: [
                        {
                            name: 'Iron Ingot',
                            id: 'iron_ingot',
                            asset_id: 'iron_ingot',
                            quantity_per_minute: 30,
                            ingredients: [
                                {
                                    asset_id: 'iron_ore',
                                    quantity_per_minute: 30
                                }
                            ]
                        },
                        {
                            name: 'Copper Ingot',
                            id: 'copper_ingot',
                            asset_id: 'copper_ingot',
                            quantity_per_minute: 30,
                            ingredients: [
                                {
                                    asset_id: 'copper_ore',
                                    quantity_per_minute: 30
                                }
                            ]
                        },
                        {
                            name: 'Caterium Ingot',
                            id: 'caterium_ingot',
                            asset_id: 'caterium_ingot',
                            quantity_per_minute: 15,
                            ingredients: [
                                {
                                    asset_id: 'caterium_ore',
                                    quantity_per_minute: 45
                                }
                            ]
                        },
                    ]
                },
                {
                    name: 'Constructor',
                    id: 'konstructor',
                    color: '#1976D2',
                    recipes: [
                        {
                            name: 'Screw 1',
                            id: 'screw_1',
                            asset_id: 'screw',
                            quantity_per_minute: 40,
                            ingredients: [
                                {
                                    asset_id: 'iron_rod',
                                    quantity_per_minute: 10
                                }
                            ]
                        },
                        {
                            name: 'Iron Plate 1',
                            id: 'iron_plate_1',
                            asset_id: 'iron_plate',
                            quantity_per_minute: 20,
                            ingredients: [
                                {
                                    asset_id: 'iron_ingot',
                                    quantity_per_minute: 30
                                }
                            ]
                        },
                        {
                            name: 'Iron Rod 1',
                            id: 'iron_rod_1',
                            asset_id: 'iron_rod',
                            quantity_per_minute: 15,
                            ingredients: [
                                {
                                    asset_id: 'iron_ingot',
                                    quantity_per_minute: 15
                                }
                            ]
                        },
                        {
                            name: 'Concrete 1',
                            id: 'concrete_1',
                            asset_id: 'concrete',
                            quantity_per_minute: 15,
                            ingredients: [
                                {
                                    asset_id: 'limestone',
                                    quantity_per_minute: 45
                                }
                            ]
                        },
                        {
                            name: 'Wire 1',
                            id: 'wire_1',
                            asset_id: 'wire',
                            quantity_per_minute: 30,
                            ingredients: [
                                {
                                    asset_id: 'copper_ingot',
                                    quantity_per_minute: 15
                                }
                            ]
                        },
                        {
                            name: 'Cable 1',
                            id: 'cable_1',
                            asset_id: 'cable',
                            quantity_per_minute: 30,
                            ingredients: [
                                {
                                    asset_id: 'wire',
                                    quantity_per_minute: 60
                                }
                            ]
                        },
                        {
                            name: 'Quickwire 1',
                            id: 'quickwire_1',
                            asset_id: 'quickwire',
                            quantity_per_minute: 60,
                            ingredients: [
                                {
                                    asset_id: 'caterium_ingot',
                                    quantity_per_minute: 12
                                }
                            ]
                        },
                        {
                            name: 'Steel Beam 1',
                            id: 'steel_beam_1',
                            asset_id: 'steel_beam',
                            quantity_per_minute: 15,
                            ingredients: [
                                {
                                    asset_id: 'steel_ingot',
                                    quantity_per_minute: 60
                                }
                            ]
                        },
                        {
                            name: 'Steel Pipe 1',
                            id: 'steel_pipe_1',
                            asset_id: 'steel_pipe',
                            quantity_per_minute: 20,
                            ingredients: [
                                {
                                    asset_id: 'steel_ingot',
                                    quantity_per_minute: 30
                                }
                            ]
                        },
                        {
                            name: 'Copper Sheet 1',
                            id: 'copper_sheet_1',
                            asset_id: 'copper_sheet',
                            quantity_per_minute: 10,
                            ingredients: [
                                {
                                    asset_id: 'copper_ingot',
                                    quantity_per_minute: 20
                                }
                            ]
                        }
                    ]
                },
                {
                    name: 'Assembler',
                    id: 'assembler',
                    color: '#388E3C',
                    recipes: [
                        {
                            name: 'Modular Frame 1',
                            id: 'modular_frame_1',
                            asset_id: 'modular_frame',
                            quantity_per_minute: 2,
                            ingredients: [
                                {
                                    asset_id: 'reinforced_iron_plate',
                                    quantity_per_minute: 3
                                },
                                {
                                    asset_id: 'iron_rod',
                                    quantity_per_minute: 12
                                }
                            ]
                        },
                        {
                            name: 'Reinforced Iron Plate 1',
                            id: 'reinforced_iron_plate_1',
                            asset_id: 'reinforced_iron_plate',
                            quantity_per_minute: 5,
                            ingredients: [
                                {
                                    asset_id: 'iron_plate',
                                    quantity_per_minute: 30
                                },
                                {
                                    asset_id: 'screw',
                                    quantity_per_minute: 60
                                }
                            ]
                        },
                        {
                            name: 'Rotor 1',
                            id: 'rotor_1',
                            asset_id: 'rotor',
                            quantity_per_minute: 5,
                            ingredients: [
                                {
                                    asset_id: 'iron_rod',
                                    quantity_per_minute: 20
                                },
                                {
                                    asset_id: 'screw',
                                    quantity_per_minute: 100
                                }
                            ]
                        },
                        {
                            name: 'Stator 1',
                            id: 'stator_1',
                            asset_id: 'stator',
                            quantity_per_minute: 5,
                            ingredients: [
                                {
                                    asset_id: 'steel_pipe',
                                    quantity_per_minute: 15
                                },
                                {
                                    asset_id: 'wire',
                                    quantity_per_minute: 40
                                }
                            ]
                        },
                        {
                            name: 'Motor 1',
                            id: 'motor_1',
                            asset_id: 'motor',
                            quantity_per_minute: 5,
                            ingredients: [
                                {
                                    asset_id: 'rotor',
                                    quantity_per_minute: 10
                                },
                                {
                                    asset_id: 'stator',
                                    quantity_per_minute: 10
                                }
                            ]
                        },
                        {
                            name: 'Smart Plating 1',
                            id: 'smart_plating_1',
                            asset_id: 'smart_plating',
                            quantity_per_minute: 2,
                            ingredients: [
                                {
                                    asset_id: 'reinforced_iron_plate',
                                    quantity_per_minute: 2
                                },
                                {
                                    asset_id: 'rotor',
                                    quantity_per_minute: 2
                                }
                            ]
                        },
                        {
                            name: 'Versatile Framework 1',
                            id: 'versatile_framework_1',
                            asset_id: 'versatile_framework',
                            quantity_per_minute: 5,
                            ingredients: [
                                {
                                    asset_id: 'modular_frame',
                                    quantity_per_minute: 2.5
                                },
                                {
                                    asset_id: 'steel_beam',
                                    quantity_per_minute: 30
                                }
                            ]
                        },
                        {
                            name: 'Automated Wiring 1',
                            id: 'automated_wiring_1',
                            asset_id: 'automated_wiring',
                            quantity_per_minute: 2.5,
                            ingredients: [
                                {
                                    asset_id: 'stator',
                                    quantity_per_minute: 2.5
                                },
                                {
                                    asset_id: 'cable',
                                    quantity_per_minute: 50
                                }
                            ]
                        },
                        {
                            name: 'Encaesed Industrial Beam 1',
                            id: 'encased_industrial_beam_1',
                            asset_id: 'encased_industrial_beam',
                            quantity_per_minute: 6,
                            ingredients: [
                                {
                                    asset_id: 'steel_beam',
                                    quantity_per_minute: 18
                                },
                                {
                                    asset_id: 'concrete',
                                    quantity_per_minute: 36
                                }
                            ]
                        },
                        {
                            name: 'Fused Quickwire 1',
                            id: 'fused_quickwire_1',
                            asset_id: 'quickwire',
                            quantity_per_minute: 90,
                            ingredients: [
                                {
                                    asset_id: 'caterium_ingot',
                                    quantity_per_minute: 7.5
                                },
                                {
                                    asset_id: 'copper_ingot',
                                    quantity_per_minute: 37.5
                                }
                            ]
                        },
                        {
                            name: 'Fine Concrete',
                            id: 'fine_concrete_1',
                            asset_id: 'concrete',
                            quantity_per_minute: 50,
                            ingredients: [
                                {
                                    asset_id: 'limestone',
                                    quantity_per_minute: 60
                                },
                                {
                                    asset_id: 'silica',
                                    quantity_per_minute: 15
                                }
                            ]
                        },
                        {
                            name: 'Black Powder 1',
                            id: 'black_powder_1',
                            asset_id: 'black_powder',
                            quantity_per_minute: 30,
                            ingredients: [
                                {
                                    asset_id: 'coal',
                                    quantity_per_minute: 15
                                },
                                {
                                    asset_id: 'sulfur',
                                    quantity_per_minute: 15
                                }
                            ]
                        },
                        {
                            name: 'Fabric 1',
                            id: 'fabric_1',
                            asset_id: 'fabric',
                            quantity_per_minute: 15,
                            ingredients: [
                                {
                                    asset_id: 'mycelia',
                                    quantity_per_minute: 15
                                },
                                {
                                    asset_id: 'biomass',
                                    quantity_per_minute: 75
                                }
                            ]
                        }
                    ]
                },
                {
                    name: 'Manufacturer',
                    id: 'manufacturer',
                    color: '#FBC02D'
                },
                {
                    name: 'Refinery',
                    id: 'refinery',
                    color: '#4CAF50',
                    recipes: [
                        {
                            name: 'Plastic 1',
                            id: 'plastic_1',
                            asset_id: 'plastic',
                            quantity_per_minute: 20,
                            ingredients: [
                                {
                                    asset_id: 'crude_oil',
                                    quantity_per_minute: 30
                                }
                            ]
                        },
                        {
                            name: 'Rubber 1',
                            id: 'rubber_1',
                            asset_id: 'rubber',
                            quantity_per_minute: 30,
                            ingredients: [
                                {
                                    asset_id: 'crude_oil',
                                    quantity_per_minute: 30
                                }
                            ]
                        },
                        {
                            name: 'Fuel 1',
                            id: 'fuel_1',
                            asset_id: 'fuel',
                            quantity_per_minute: 40,
                            ingredients: [
                                {
                                    asset_id: 'crude_oil',
                                    quantity_per_minute: 60
                                }
                            ]
                        }
                    ]
                },
                {
                    name: 'Foundry',
                    id: 'foundry',
                    color: '#F57C00',
                    recipes: [
                        {
                            name: 'Steel Ingot 1',
                            id: 'steel_ingot_1',
                            asset_id: 'steel_ingot',
                            quantity_per_minute: 45,
                            ingredients: [
                                {
                                    asset_id: 'iron_ore',
                                    quantity_per_minute: 45
                                },
                                {
                                    asset_id: 'coal',
                                    quantity_per_minute: 45
                                }
                            ]
                        }
                    ]
                },
                {
                    name: 'Water Extractor',
                    id: 'water_extractor',
                    color: '#0288D1',
                    recipes: [
                        {
                            name: 'Water 1',
                            id: 'water_1',
                            asset_id: 'water',
                            quantity_per_minute: 120,
                            ingredients: []
                        }
                    ]
                },
                {
                    name: 'Coal-Powered Generator',
                    id: 'coal_powered_generator',
                    color: '#0E1419',
                    recipes: [
                        {
                            name: 'Power 1',
                            id: 'power_1',
                            asset_id: 'power',
                            quantity_per_minute: 75,
                            ingredients: [
                                {
                                    asset_id: 'coal',
                                    quantity_per_minute: 15
                                },
                                {
                                    asset_id: 'water',
                                    quantity_per_minute: 45
                                }
                            ]
                        }
                    ]
                },
                {
                    name: 'Oil Extractor (Normal)',
                    id: 'oil_extractor',
                    color: '#D81B60',
                    recipes: [
                        {
                            name: 'Crude Oil 1',
                            id: 'crude_oil_1',
                            asset_id: 'crude_oil',
                            quantity_per_minute: 120,
                            ingredients: []
                        }
                    ]
                },
                {
                    name: 'Miner',
                    id: 'miner',
                    color: '#5D4037',
                    recipes: [
                        {
                            name: 'Iron Ore MK2',
                            id: 'iron_ore_1',
                            asset_id: 'iron_ore',
                            quantity_per_minute: 240,
                            ingredients: []
                        },
                        {
                            name: 'Copper Ore MK2',
                            id: 'copper_ore_1',
                            asset_id: 'copper_ore',
                            quantity_per_minute: 240,
                            ingredients: []
                        },
                        {
                            name: 'Limestone MK2',
                            id: 'limestone_1',
                            asset_id: 'limestone',
                            quantity_per_minute: 240,
                            ingredients: []
                        },
                        {
                            name: 'Coal MK2',
                            id: 'coal_1',
                            asset_id: 'coal',
                            quantity_per_minute: 240,
                            ingredients: []
                        },
                        {
                            name: 'Caterium Ore MK2',
                            id: 'caterium_ore_1',
                            asset_id: 'caterium_ore',
                            quantity_per_minute: 240,
                            ingredients: []
                        },
                        {
                            name: 'Sulfur MK2',
                            id: 'sulfur_1',
                            asset_id: 'sulfur',
                            quantity_per_minute: 240,
                            ingredients: []
                        },
                        {
                            name: 'Bauxite MK2',
                            id: 'bauxite_1',
                            asset_id: 'bauxite',
                            quantity_per_minute: 240,
                            ingredients: []
                        },
                        {
                            name: 'Quartz MK2',
                            id: 'quartz_1',
                            asset_id: 'quartz',
                            quantity_per_minute: 240,
                            ingredients: []
                        },
                    ]
                },
                {
                    name: 'Oil Pump',
                    id: 'oil_pump',
                    color: '#FFC107'
                }
            ],
            resource_types: [
                {
                    name: 'Iron Ore',
                    id: 'iron_ore'
                },
                {
                    name: 'Copper Ore',
                    id: 'copper_ore'
                },
                {
                    name: 'Limestone',
                    id: 'limestone'
                },
                {
                    name: 'Coal',
                    id: 'coal'
                },
                {
                    name: 'Caterium Ore',
                    id: 'caterium_ore'
                },
                {
                    name: 'Sulfur',
                    id: 'sulfur'
                },
                {
                    name: 'Bauxite',
                    id: 'bauxite'
                },
                {
                    name: 'Quartz',
                    id: 'quartz'
                },
                {
                    name: 'Oil',
                    id: 'oil'
                },
                {
                    name: 'Water',
                    id: 'water'
                }
            ],
            material_types: [
                {
                    name: 'Iron Ingot',
                    id: 'iron_ingot'
                },
                {
                    name: 'Copper Ingot',
                    id: 'copper_ingot'
                },

                {
                    name: 'Caterium Ingot',
                    id: 'caterium_ingot'
                },
                {
                    name: 'Steel Ingot',
                    id: 'steel_ingot'
                },
                {
                    name: 'Concrete',
                    id: 'concrete'
                },
                {
                    name: 'Wire',
                    id: 'wire'
                },
                {
                    name: 'Cable',
                    id: 'cable'
                },
                {
                    name: 'Quickwire',
                    id: 'quickwire'
                },
                {
                    name: 'Screw',
                    id: 'screw'
                },
                {
                    name: 'Iron Plate',
                    id: 'iron_plate'
                },
                {
                    name: 'Iron Rod',
                    id: 'iron_rod'
                },
                {
                    name: 'Reinforced Iron Plate',
                    id: 'reinforced_iron_plate'
                },
                {
                    name: 'Rotor',
                    id: 'rotor'
                },
                {
                    name: 'Modular Frame',
                    id: 'modular_frame'
                },
                {
                    name: 'Encased Industrial Beam',
                    id: 'encased_industrial_beam'
                },
                {
                    name: 'Steel Beam',
                    id: 'steel_beam'
                },
                {
                    name: 'Steel Pipe',
                    id: 'steel_pipe'
                },
                {
                    name: 'Plastic',
                    id: 'plastic'
                },
                {
                    name: 'Rubber',
                    id: 'rubber'
                },
                {
                    name: 'Crystal Oscillator',
                    id: 'crystal_oscillator'
                },
                {
                    name: 'Circuit Board',
                    id: 'circuit_board'
                },
                {
                    name: 'Heat Sink',
                    id: 'heat_sink'
                },
                {
                    name: 'Radio Control Unit',
                    id: 'radio_control_unit'
                },
                {
                    name: 'Battery',
                    id: 'battery'
                },
                {
                    name: 'Electromagnetic Control Rod',
                    id: 'electromagnetic_control_rod'
                },
                {
                    name: 'Crystal Beacon',
                    id: 'crystal_beacon'
                },
                {
                    name: 'Nobelisk',
                    id: 'nobelisk'
                },
                {
                    name: 'Explosive',
                    id: 'explosive'
                },
                {
                    name: 'Black Powder',
                    id: 'black_powder'
                },
                {
                    name: 'Color Cartridge',
                    id: 'color_cartridge'
                },
                {
                    name: 'Fabric',
                    id: 'fabric'
                },
                {
                    name: 'Plastic Smart Plating',
                    id: 'plastic_smart_plating'
                },
                {
                    name: 'Versatile Framework',
                    id: 'versatile_framework'
                },
                {
                    name: 'Automated Wiring',
                    id: 'automated_wiring'
                },
                {
                    name: 'Modular Engine',
                    id: 'modular_engine'
                },
                {
                    name: 'Adaptive Control Unit',
                    id: 'adaptive_control_unit'
                },
                {
                    name: 'Supercomputer',
                    id: 'supercomputer'
                },
                {
                    name: 'High-Speed Connector',
                    id: 'high_speed_connector'
                },
                {
                    name: 'Turbo Motor',
                    id: 'turbo_motor'
                },
                {
                    name: 'Motor',
                    id: 'motor'
                },
                {
                    name: 'Radio Control System',
                    id: 'radio_control_system'
                },
                {
                    name: 'Silica',
                    id: 'silica'
                },
                {
                    name: 'Alumina Solution',
                    id: 'alumina_solution'
                },
                {
                    name: 'Aluminum Scrap',
                    id: 'aluminum_scrap'
                },
                {
                    name: 'Smart Plating',
                    id: 'smart_plating'
                },
                {
                    name: 'Power',
                    id: 'power'
                },
                {
                    name: 'Stator',
                    id: 'stator'
                }
            ],
            global_assets_dictionary: null,
        }
    },
    created() {
        console.log('Source Data Created')
        this.global_assets_dictionary = this.machine_types.map((item) => { item.type = 'machine'; return item })
            .concat(this.resource_types.map((item) => { item.type = 'resource'; return item }))
            .concat(this.material_types.map((item) => { item.type = 'material'; return item }))
            .reduce((acc, item) => {
                acc[item.id] = item
                return acc
            }, {});
    },
}
</script>
